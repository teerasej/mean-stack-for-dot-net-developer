
# Implement Route: find by Id, update, delete user, find by username

เปิดไฟล์ `routes/users.js` 

## 1. Implement find by ID

```js
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
```

## 2. Implement update user

```js
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

    // update user ด้วย username และ password
    let result = await UsersModel.updateOne(
        { _id: targetUser.id },
        { username: targetUser.username, password: targetUser.password }
    )
    res.json(result);
   
})
```

## 3. Implement remove user

```js
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

    // ลบ user จาก id ที่ได้
    let result = await UsersModel.findByIdAndDelete(targetUser.id)
    res.json(result);
})
```

## 4. Implement find by username 


```js
router.get('/username/:username', isAuthenticated, async (req, res) => {

    const joiSchema = Joi.string().required().min(1);

    const { error, value } = joiSchema.validate(req.params.username);
    if(error) {
        res.status(422).json({
            message: 'Invalid request',
            data: error
        })
    }
    // ค้นหา user จาก username
    let result = await UsersModel.findOne({ username: req.params.username })
    // console.log(result);
    res.json(result);

});
```

## A. ไฟล์เต็ม `routes/users.js`

```js
import 'babel-polyfill';
import express from 'express';
import { isAuthenticated } from "../jwt/JWT";
import UsersModel from "../models/UsersModel";
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


export default router;
```