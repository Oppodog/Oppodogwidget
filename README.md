# Oppo Jupiter Ultra Swap Widget

Modern, mobile-responsive decentralized token swap widget powered by Jupiter Ultra API with integrated referral fee system.

## Features

### 🚀 Jupiter Ultra API Integration
- **Referral Fee System**: 0.2% fee on supported token swaps
- **Fee Distribution**: 80% protocol, 20% Jupiter (as per Jupiter Ultra specification)
- **Real-time Processing**: Automated fee collection and distribution
- **Referral Account**: `9EvV3V9cZ4KktQ4xCnu3ymA2a9qgaBR4HLFhFddZZXSn`

### 💰 Supported Tokens
The widget applies referral fees to these premium Solana tokens:
- **SOL** - Native Solana
- **USDC** - USD Coin
- **USDT** - Tether USD  
- **JUP** - Jupiter
- **RAY** - Raydium
- **mSOL** - Marinade SOL
- **JupSOL** - Jupiter SOL
- **SSOL** - Solana Staked SOL
- **JitoSOL** - Jito SOL
- **ETH** - Ethereum (Wormhole)
- **EURC** - Euro Coin
- **USDG** - USDG Token
- **PUMP** - PUMP Token

### ⚡ Ultra Features
- Real-time fee tracking
- Unclaimed fee monitoring
- $1 minimum claim threshold
- Verified / non-verified claim logic
- Mobile & desktop optimized UI
- Graceful fallback (demo mode) for local/offline usage

## Technical Implementation

### Configuration
```javascript
const REFERRAL_CONFIG = {
  feeBps: 20, // 0.2%
  feeRecipient: "9EvV3V9cZ4KktQ4xCnu3ymA2a9qgaBR4HLFhFddZZXSn",
  ultraFeeShare: { protocol: 80, jupiter: 20 }
};
```

### Fee Processing Flow
1. User executes swap
2. Transaction succeeds
3. Token mints checked (supported list)
4. Referral fee handler invoked
5. Jupiter Ultra processes distribution
6. Status surfaced to user

### Claim System
- Tracks unclaimed fees
- Auto-claim if ≥ $1 and verified
- Demo mode simulates response locally

## Quick Start
```bash
# Clone repo
git clone https://github.com/Oppodog/Oppodog.git
cd Oppodog

# (Statik kullanım) index.html dosyasını aç
# veya küçük bir HTTP sunucusu:
python3 -m http.server 8000
# Aç: http://localhost:8000
```

## Embed Example
```html
<script src="https://plugin.jup.ag/plugin-v1.js"></script>
<div id="target-container"></div>
<script>
window.Jupiter.init({
  displayMode: "integrated",
  integratedTargetId: "target-container",
  feeBps: 20,
  feeRecipient: "9EvV3V9cZ4KktQ4xCnu3ymA2a9qgaBR4HLFhFddZZXSn",
  branding: {
    logoUri: "https://photos.pinksale.finance/file/pinksale-logo-upload/1733923962272-6c08b5b4359a38ef4991bd3d69dc1c3d.png",
    name: "Oppo"
  }
});
</script>
```

## API Helpers (Exposed on window.OppoConfig)
- handleUltraReferralFee(txSignature)
- checkUnclaimedFees()
- isTokenSupported(mint)
- getTokenSymbol(mint)
- formatFeeAmount(amount, decimals)

## Roadmap (öneri)
- [ ] Wallet adapter entegrasyonu
- [ ] Gerçek swap flow + route optimizasyonu
- [ ] Dinamik token listesi & keşif
- [ ] Gelişmiş hata & loading durumu
- [ ] CI (lint + test) pipeline
- [ ] Test altyapısı (Vitest / RTL)

## Docs
- [Jupiter Ultra API](https://dev.jup.ag/docs/ultra-api/add-fees-to-ultra)
- [Widget Docs](https://docs.jup.ag/integrate/widget)

## License
MIT