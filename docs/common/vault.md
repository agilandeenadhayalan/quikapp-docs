---
sidebar_position: 7
---

# Vault Integration

HashiCorp Vault for secrets management.

## Features

- Multiple auth methods (Token, AppRole, Kubernetes)
- KV v2 secrets engine
- Dynamic database credentials
- Transit encryption/decryption
- Secret caching

## Usage

```typescript
@Injectable()
export class SecretService {
  constructor(private vault: VaultService) {}

  async getDatabaseCredentials() {
    return this.vault.getDatabaseCredentials('quckchat-role');
  }

  async encryptSensitiveData(data: string) {
    return this.vault.encrypt('quckchat-key', data);
  }
}
```

## Secret Paths

```typescript
export const VAULT_PATHS = {
  APP_SECRETS: 'secret/data/quckchat',
  DATABASE: 'secret/data/quckchat/database',
  JWT: 'secret/data/quckchat/jwt',
  FIREBASE: 'secret/data/quckchat/firebase',
  SMTP: 'secret/data/quckchat/smtp',
};
```
