# edu-auth

# Instructions 

```bash
mkdir auth-server && cd auth-server
mkdir src
mkdir ./src/{routes,controller,domain}
mkdir ./__tests__
touch ./src/{config.js,app.js,service.js}
npm install jsonwebtoken dotenv
npm install -D jest jest-runner-groups supertest
npm pgk set scripts.main='node ./service.js'
npm pgk set scripts.dev='node --watch ./service.js'
npm pgk set scripts.test='jest'
npm pkg set scripts.test="jest  --group=-component --group=-integration"
npm pkg set scripts.test:component="jest --group=component"
npm pkg set scripts.test:integration="jest --group=integration"
npm pkg set jest.runner="groups"
```

## Tests

### unit_tests.js <heredoc

```js
cat > ./__tests__/unit_tests.js << 'EOF'
/**
 * @group unit
 */

describe('jest', () => {
  describe('unit test', () => {
    it('should work', () => {
      expect(true).toBe(true);
    });
  });
});
EOF
```

### component_tests.js <dochere

```js
cat > ./__tests__/component_tests.js << 'EOF'
/**
 * @group component
 */

describe('jest', () => {
  describe('component test', () => {
    it('should work', () => {
      expect(true).toBe(true);
    });
  });
});
EOF
```

### integration_tests.js <docere

```js
cat > ./__tests__/integration_tests.js << 'EOF'
/**
 * @group integration
 */

describe('jest', () => {
  describe('integration test', () => {
    it('should work', () => {
      expect(true).toBe(true);
    });
  });
});
EOF
```
