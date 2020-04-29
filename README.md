# Uso de JWT en NodeJS

### Instalación
```bash
npm install express

npm install jsonwebtoken
```

### Código
```javascript
const express = require('express');
const jwt = require('jsonwebtoken');

const secretKey = '77a7b6788f96e0fa3ce1b24e79e8f6a9cd85c7e85ddd8b7b27503e33d1705930';

const app = express();


app.get('/api', (req, res) => {
    res.json({
        message: 'Lorem Ipsum'
    });
});


app.post('/api/post', checkToken, (req, res) => {
    jwt.verify(req.token, secretKey, (err, authData) => {
        if (err) {
            res.sendStatus(403);
        } else {
            res.json({
                message: 'POST ok',
                authData
            });
        }
    });
});


app.post('/api/login', (req, res) => {
    const user = {
        id: 1,
        username: 'iga',
        email: 'iga@eleus.com'
    }

    jwt.sign({
        user
    }, secretKey, {
        expiresIn: '60s'
    }, (err, token) => {
        res.json({
            token
        });
    });
});


function checkToken(req, res, next) {
    const header = req.headers['authorization'];

    if (typeof header !== 'undefined') {
        const bearer = header.split(' ');
        const token = bearer[1];
        req.token = token;
        next();
    } else {
        res.sendStatus(403);
    }
}


app.listen(3000, () => console.log('Server on port 3000'));
```


### Ejecución
```bash
node index.js
```

### Test
GET [http://localhost:3000/api](http://localhost:3000/api)

### Login (mockeado)
POST [http://localhost:3000/api/login](http://localhost:3000/api/login)

### Test
POST [http://localhost:3000/api/post](http://localhost:3000/api/post)

> utilizar autenticación bearer


### Nota
La secret key ha sido generada en [keygen.io](https://keygen.io/)