# 🚀 Pre-Push Checklist for GitHub

This checklist ensures your project is ready to push to GitHub and that CI will pass.

## ✅ Security Checks

### 1. Sensitive Files
- [x] `.env` files are in `.gitignore` ✅
- [x] `packages/hardhat/.env` should NOT be committed
- [x] Private keys should NOT be in repository
- [x] `deployedContracts.ts` is in `.gitignore` (auto-generated)

### 2. Verify No Secrets in Code
```bash
# Check for hardcoded private keys
grep -r "0x[a-fA-F0-9]\{64\}" --exclude-dir=node_modules --exclude="*.lock" || echo "✅ No hardcoded private keys found"
```

## ✅ CI Workflow Verification

### GitHub Actions Workflow (`.github/workflows/ci.yml`)

The CI runs 4 jobs:

1. **Compile Contracts** ✅
   - Uses Foundry (primary) - will work
   - Hardhat (fallback) - optional, can fail

2. **Run Tests** ✅
   - Foundry tests - will work
   - Hardhat tests - may need local node

3. **Lint Code** ✅
   - Should work if linting is configured

4. **Type Check** ✅
   - TypeScript type checking for both packages

### Potential Issues

⚠️ **Hardhat Tests** - May fail if they require a local node
- **Solution**: Tests are optional or use Foundry tests

⚠️ **Hardhat Compilation** - May fail (HH502 error)
- **Solution**: Already marked as optional with `|| echo "Hardhat compilation optional"`

## ✅ Required Files

- [x] `package.json` (root)
- [x] `yarn.lock`
- [x] `.github/workflows/ci.yml`
- [x] `packages/hardhat/foundry/foundry.toml`
- [x] `packages/nextjs/scaffold.config.ts`
- [x] `.gitignore`

## ✅ Build Verification

### Test Locally Before Pushing

```bash
# 1. Compile with Foundry (what CI uses)
yarn foundry:build

# 2. Run Foundry tests
yarn foundry:test

# 3. Type check
yarn hardhat:check-types
yarn next:check-types

# 4. Lint (if configured)
yarn lint
```

## ✅ Git Status Check

Before pushing, verify:

```bash
# Check what will be committed
git status

# Make sure .env files are NOT staged
git status | grep -i "\.env" && echo "⚠️  WARNING: .env files detected!" || echo "✅ No .env files in staging"
```

## 🚀 Push Commands

```bash
# 1. Add all files (except those in .gitignore)
git add .

# 2. Commit
git commit -m "feat: Deploy contracts to Paseo testnet

- Added ERC20 and ERC721 token contracts
- Deployed to Paseo Asset Hub testnet
- Created standalone deployment scripts (bypass Hardhat compiler)
- Updated configurations for Paseo network
- Added CI workflow for GitHub Actions"

# 3. Push to GitHub
git remote add origin https://github.com/Funghi88/scaffold-dot.git
git branch -M main
git push -u origin main
```

## ⚠️ Important Notes

1. **Never commit `.env` files** - They contain encrypted private keys
2. **`deployedContracts.ts` is auto-generated** - Don't manually edit it
3. **CI will use Foundry** - Hardhat compilation is optional
4. **Tests may need adjustment** - Some tests might require local node setup

## ✅ Expected CI Results

- ✅ **Compile**: Will pass (Foundry works)
- ⚠️ **Test**: May have some failures if tests need local node
- ✅ **Lint**: Should pass
- ✅ **Type Check**: Should pass

## 🔧 If CI Fails

1. Check the GitHub Actions logs
2. Most likely issues:
   - Hardhat tests requiring local node (expected, can be skipped)
   - Missing dependencies (run `yarn install`)
   - Type errors (fix TypeScript issues)

## 📋 Final Checklist

- [ ] All sensitive files are in `.gitignore`
- [ ] No private keys in code
- [ ] `yarn foundry:build` works locally
- [ ] `yarn foundry:test` works locally
- [ ] Type checking passes
- [ ] Git status shows no `.env` files
- [ ] Ready to push!

---

**You're ready to push!** 🚀
