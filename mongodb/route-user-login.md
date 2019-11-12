
# สร้างส่วน Route Login

เริ่มจากการ import ส่วนที่ต้องใช้งานจาก module ต่างๆ 

เปิดไฟล์ `routes/users.js`

```js
import 'babel-polyfill';
import express from 'express';
// นำเข้า generateToken function เพื่อใช้ในการสร้าง token ให้ user ที่ login เข้ามาในระบบ
import { isAuthenticated, generateToken } from "../jwt/JWT";
import UsersModel from "../models/UsersModel";
// นำเข้า isUserExist function เพื่อตรวจสอบ username และ password ในฐานข้อมูล
import { isUserExist } from "../models/UsersService";
import Joi from "@hapi/joi";
```

จากนั้นสร้าง route POST /login 


```js
router.post('/login', async (req, res) => {

    const loggingInUser = req.body;

    const joiSchema = Joi.object().keys({
        username: Joi.string().required().min(1),
        password: Joi.string().required().min(1)
    })

    const { error, value } = joiSchema.validate(loggingInUser);
    if(error) {
        res.status(422).json({
            message: 'Invalid request',
            data: error
        });
    }

    const userExist = await isUserExist(loggingInUser.username, loggingInUser.password);

    if(!userExist){
        res.status(404).json({
            message: 'User not found'
        });
    } else {
        let token = generateToken(loggingInUser);
        res.json({ token: token });
    }

});
```

## A. ไฟล์เต็ม `routes/users.js`

```js
import 'babel-polyfill';
import express from 'express';
import { isAuthenticated, generateToken } from "../jwt/JWT";
import UsersModel from "../models/UsersModel";
import { isUserExist } from "../models/UsersService";
import Joi from "@hapi/joi";

const router = express.Router();

router.post('/register', async (req, res) => {

    const newUser = req.body;

    const userValidateJoi = Joi.object().keys({
        username: Joi.string().required().min(1),
        password: Joi.string().required().min(1)
    });

    const validateResult = userValidateJoi.validate(newUser);
    const { error, value } = validateResult;

    if(error) {
        res.status(422).json({
            message: 'Invalid request',
            data: error
        })
    }

    // console.log('newUser', newUser);

    let userModel = new UsersModel();
    userModel.username = req.body.username;
    userModel.password = req.body.password;
    let result = await userModel.save();

    res.json({ uid: result.id });
});

router.get('/', isAuthenticated, async (req, res) => {

    let result = await UsersModel.find();
    res.json(result);
    
});

router.get('/:id', isAuthenticated, async (req, res) => {

    const joiSchema = Joi.object().keys({
        id: Joi.string().required()
    })

    const { error, value } = joiSchema.validate(req.body);
    if(error) {
        res.status(422).json({
            message: 'Invalid request',
            data: error
        })
    }

    let result = await UsersModel.findById(req.body.id);
    res.json(result);

});
 
router.put('/', async (req, res) => {

    const targetUser = req.body;

    const joiSchema = Joi.object().keys({
        id: Joi.string().required(),
        username: Joi.string().required().min(1),
        password: Joi.string().required().min(1)
    })

    const { error, value } = joiSchema.validate(targetUser);
    if(error) {
        res.status(422).json({
            message: 'Invalid request',
            data: error
        })
    }

    let result = await UsersModel.updateOne(
        { _id: targetUser.id },
        { username: targetUser.username, password: targetUser.password }
    )
    res.json(result);
   

})

router.delete('/', async (req, res) => {
    
    const targetUser = req.body;

    const joiSchema = Joi.object().keys({
        id: Joi.string().required()
    })

    const { error, value } = joiSchema.validate(targetUser);
    if(error) {
        res.status(422).json({
            message: 'Invalid request',
            data: error
        })
    }

    let result = await UsersModel.findByIdAndDelete(targetUser.id)
    res.json(result);
})

router.get('/username/:username', isAuthenticated, async (req, res) => {

    const joiSchema = Joi.string().required().min(1);

    const { error, value } = joiSchema.validate(req.params.username);
    if(error) {
        res.status(422).json({
            message: 'Invalid request',
            data: error
        })
    }
    let result = await UsersModel.findOne({ username: req.params.username })
    // console.log(result);
    res.json(result);

});

router.post('/login', async (req, res) => {

    const loggingInUser = req.body;

    const joiSchema = Joi.object().keys({
        username: Joi.string().required().min(1),
        password: Joi.string().required().min(1)
    })

    const { error, value } = joiSchema.validate(loggingInUser);
    if(error) {
        res.status(422).json({
            message: 'Invalid request',
            data: error
        });
    }

    const userExist = await isUserExist(loggingInUser.username, loggingInUser.password);

    if(!userExist){
        res.status(404).json({
            message: 'User not found'
        });
    } else {
        let token = generateToken(loggingInUser);
        res.json({ token: token });
    }

});


export default router;
```