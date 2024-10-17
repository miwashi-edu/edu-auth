# edu-auth

# Instructions 

## Prepare Project

```bash
mkdir auth-server && cd auth-server
mkdir -p src
mkdir -p ./src/{routes,controllers,domain}
mkdir -p ./__tests__
touch ./src/{config.js,app.js,service.js}
touch ./routes/auth_routes.js
touch ./controllers/auth_controller.js
touch ./domain/auth_handler.js
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
const { PORT } = require('./config');
const app =require('./app');


app.listen(PORT, err => {
    if (err) {
        console.error(`Failed to start the server: ${err}`);
    } else {
        console.log(`Auth Server Listening on port ${PORT}`);
    }
});
EOF
```

## Config <heredoc

```bash
mkdir -p ./src
cat > ./src/config.js << 'EOF'
EOF
```

## App <heredoc


```bash
mkdir -p ./src
cat > ./src/app.js << 'EOF'
const express = require('express');
const app = express();

app.use(express.json());
app.use((req,res,next)=>{
    res.send({});
})

module.exports = app;
EOF
```

## Routes <heredoc


```bash
mkdir -p ./src/routes
cat > ./src/routes/auth_routes.js << 'EOF'
EOF
```

## Controller <heredoc
```bash
mkdir -p ./src/controllers
cat > ./src/controllers/auth_controller.js << 'EOF'
EOF
```

## Domain <heredoc
```bash
mkdir -p ./src/domain
cat > ./src/domain/auth_handler.js << 'EOF'
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
          .get('/api/user/')
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
      const res = await container.get('/api/user/');
      expect(res.statusCode).toEqual(200);
    });
  });
});

EOF
```
