# edu-auth

# Instructions 

## Prepare Project

```bash
mkdir auth-server && cd auth-server
mkdir -p src
mkdir -p ./src/{routes,controller,domain}
mkdir -p ./__tests__
touch ./src/{config.js,app.js,service.js}
```

## Prepare Package

```bash
npm init -y
npm install jsonwebtoken dotenv
npm install -D jest jest-runner-groups supertest
npm pkg set scripts.start='node ./service.js'
npm pkg set scripts.dev='node --watch ./service.js'
npm pkg set scripts.test="jest  --group=-component --group=-integration"
npm pkg set scripts.test:component="jest --group=component"
npm pkg set scripts.test:integration="jest --group=integration"
npm pkg set jest.runner="groups"
```

## Service

```bash
mkdir -p ./src
cat > ./src/service.js << 'EOF'
EOF
```

## Config
mkdir -p ./src
cat > ./src/config.js << 'EOF'
EOF

## App


```bash
mkdir -p ./src
cat > ./src/app.js << 'EOF'
EOF
```

## Routes


```bash
mkdir -p ./src/routes
cat > ./src/routes/auth_routes.js << 'EOF'
EOF
```

## Controller
```bash
mkdir -p ./src/controller
cat > ./src/controller/auth_controller.js << 'EOF'
EOF
```

## Domain
```bash
mkdir -p ./src/domain
cat > ./src/domain/auth_handler.js << 'EOF'
EOF
```

## Tests

### unit_tests.js <heredoc

```bash
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

```bash
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

```bash
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
