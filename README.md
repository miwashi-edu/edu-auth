# edu-auth

# Instructions 

## Prepare Project

```bash
mkdir auth-server && cd auth-server
mkdir -p src
mkdir -p ./src/{routes,controllers,domain}
mkdir -p ./__tests__
touch ./src/{config.js,app.js,service.js}
touch ./src/routes/auth_routes.js
touch ./src/routes/basic_auth_routes.js
touch ./src/routes/bearer_auth_routes.js
touch ./src/routes/custom_auth_routes.js
touch ./src/routes/digest_auth_routes.js
touch ./src/controllers/auth_controller.js
touch ./src./domain/auth_handler.js
touch .env .env.test
```

## Prepare Package

```bash
npm init -y
npm install express jsonwebtoken dotenv
npm install -D jest jest-runner-groups supertest
npm pkg set scripts.start='node ./src/service.js'
npm pkg set scripts.dev='node --watch ./src/service.js'
npm pkg set scripts.test="jest  --group=-component --group=-integration"
npm pkg set scripts.test:component="jest --group=component"
npm pkg set scripts.test:integration="jest --group=integration"
npm pkg set jest.runner="groups"
```

## Service <heredoc

```bash
mkdir -p ./src
cat > ./src/service.js << 'EOF'
const { PORT, AUTH, AUTH_TYPES, HTTP_ONLY, SECURE, SAME_SITE} = require('./config');
const app =require('./app');


app.listen(PORT, err => {
    if (err) {
        console.error(`Failed to start the server: ${err}`);
    } else {
        console.log(`Auth Server Listening on port ${PORT}`);
        console.log(`Using ${AUTH} authentication!`);
        console.log(`HTTPOnly is ${HTTP_ONLY}!`);
        console.log(`Secure is ${SECURE}!`);
        console.log(`Same Site is ${SAME_SITE}!`);
    }
});
EOF
```

## Config <heredoc

```bash
mkdir -p ./src
cat > ./src/config.js << 'EOF'
require('dotenv').config();

const ACCESS_TOKEN_SECRET = process.env.ACCESS_TOKEN_SECRET || 'your_access_token_secret_here';
const REFRESH_TOKEN_SECRET = process.env.REFRESH_TOKEN_SECRET || 'your_refresh_token_secret_here';

const AUTH_TYPES = {
    BASIC: "Basic",
    BEARER: "Bearer",
    DIGEST: "Digest",
    CUSTOM: "Custom",
    NONE: "None"
};

const SAME_SITE_TYPES = {
    STRICT: "Strict",
    LAX: "Lax",
    NONE: "None"
};

const PORT = process.env.PORT || 3000;
const HOST = process.env.HOST || 'localhost';
const SECURE = process.env.NODE_ENV === 'production';
const HTTP_ONLY = process.env.HTTP_ONLY || false;

const AUTH = AUTH_TYPES[(process.env.AUTH || 'NONE').toUpperCase()] || AUTH_TYPES.NONE;
const SAME_SITE = SAME_SITE_TYPES[(process.env.SAME_SITE || 'NONE').toUpperCase()] || SAME_SITE_TYPES.NONE;

module.exports = {HOST,PORT,SECURE,HTTP_ONLY,AUTH,AUTH_TYPES,SAME_SITE,SAME_SITE_TYPES,ACCESS_TOKEN_SECRET,REFRESH_TOKEN_SECRET};
EOF
```

## App <heredoc


```bash
mkdir -p ./src
cat > ./src/app.js << 'EOF'
const express = require('express');
const { AUTH, AUTH_TYPES } = require('./config');
const app = express();
app.use(express.json());

app.get('/', (req,res) => {
    res.send({});
});

const authRouter = {
    [AUTH_TYPES.BASIC]: require('./routes/basic_auth_routes'),
    [AUTH_TYPES.BEARER]: require('./routes/bearer_auth_routes'),
    [AUTH_TYPES.DIGEST]: require('./routes/digest_auth_routes'),
    [AUTH_TYPES.CUSTOM]: require('./routes/custom_auth_routes'),
    [AUTH_TYPES.NONE]: require('./routes/auth_routes'),
}[AUTH] || require('./routes/defaultAuthRouter');

app.use('/api/auth', authRouter);
app.use((req, res) => res.status(404).send('Not Found'));

module.exports = app;
EOF
```

## Routes <heredoc


```bash
mkdir -p ./src/routes
cat > ./src/routes/auth_routes.js << 'EOF'
const router = require('express').Router();
const authController = require("../controllers/auth_controller");

router.post('/login', authController.nologinLogin);
router.get('/login', authController.nologinLogin);

module.exports = router;
EOF
```

## Controller <heredoc
```bash
mkdir -p ./src/controllers
cat > ./src/controllers/auth_controller.js << 'EOF'
const {SECURE, HTTP_ONLY, SAME_SITE } = require("../config");
const {generateAccessToken, generateRefreshToken, generateCsrfToken}  = require('../domain/auth_handler');
exports.basicLogin = (req, res) => {
    res.status(200).send("Basic login successful");
};

exports.bearerLogin = (req, res) => {
    res.status(200).send("Bearer token provided");
};

exports.refreshToken = (req, res) => {
    res.status(200).send("Bearer token refreshed");
};

exports.digestLogin = (req, res) => {
    res.status(200).send("Digest login successful");
};

exports.customLogin = (req, res) => {
    res.status(200).send("Custom login successful");
};

exports.nologinLogin = (req, res) => {
    const user = req.body || { user: 'user@example.com' };

    const accessToken = generateAccessToken(user);
    const refreshToken = generateRefreshToken(user);
    const csrfToken = generateCsrfToken();

    res.cookie('accessToken', accessToken, {
        httpOnly: HTTP_ONLY,
        secure: SECURE,
        maxAge: 15 * 60 * 1000,
        sameSite: SAME_SITE
    });

    res.cookie('refreshToken', refreshToken, {
        httpOnly: HTTP_ONLY,
        secure: SECURE,
        maxAge: 7 * 24 * 60 * 60 * 1000,
        path: '/refresh',
        sameSite: SAME_SITE
    });

    res.json({ csrfToken });
}

exports.logout = (req, res) => {
    res.status(200).send("Logout successful");
}
EOF
```

## Domain <heredoc
```bash
mkdir -p ./src/domain
cat > ./src/domain/auth_handler.js << 'EOF'
const {ACCESS_TOKEN_SECRET, REFRESH_TOKEN_SECRET} = require('../config');

const jwt = require('jsonwebtoken');
const crypto = require('crypto');

const generateAccessToken = (user) => {
    return jwt.sign(user, ACCESS_TOKEN_SECRET, { expiresIn: '15m' });
};

const generateRefreshToken = (user) => {
    return jwt.sign(user, REFRESH_TOKEN_SECRET, { expiresIn: '7d' });
};

const generateCsrfToken = () => {
    return crypto.randomBytes(16).toString('hex');
};

module.exports = { generateAccessToken, generateRefreshToken, generateCsrfToken };
EOF
```

## Tests

### unit_tests.js <heredoc

```bash
/**
 * @group unit
 */

require('dotenv').config({ path: './.env.test' });

describe('jest', () => {
  describe('unit test', () => {
    it('should work', () => {
      expect(true).toBe(true);
    });
  });
});
EOF
```

### component_tests.js <heredoc

```bash
cat > ./__tests__/component_tests.js << 'EOF'
/**
 * @group component
 */
require('dotenv').config({ path: './.env.test' });
const request = require('supertest')
const app = require('../src/app')

describe('When testing /', () => {
  describe('GET', () => {
    it('should work', async () => {
      const res = await request(app)
          .get('/')
      expect(res.statusCode).toEqual(200);
    });
  });
});
EOF
```

### integration_tests.js <heredoc

```bash
cat > ./__tests__/integration_tests.js << 'EOF'
/**
 * @group integration
 */
require('dotenv').config({ path: './.env.test' });
const request = require('supertest')
const PORT = process.env.PORT || 3000;
const HOST = process.env.HOST || `http://localhost:${PORT}`;
const container = request(HOST);

describe('When testing /', () => {
  describe('GET', () => {
    it('should work', async () => {
      const res = await container.get('/');
      expect(res.statusCode).toEqual(200);
    });
  });
});

EOF
```
