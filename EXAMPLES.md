# 💡 PRAKTICKÉ PŘÍKLADY - Co všechno zvládá Crypto AI

Zde jsou konkrétní příklady jak používat AI s promptem.

---

## 📚 PŘÍKLADY OTÁZEK

### KATEGORIE 1: ZÁKLADY BLOCKCHAINŮ

#### Otázka:
```
Vysvětli rozdíl mezi Ethereem a Bitcoinem
```

#### Co AI odpovídá:
- Bitcoin: peer-to-peer cash, PoW, jednoduché transakce
- Ethereum: platforma na smlouvy, PoS, EVM, komplexnější
- Rozdíly v konsenzusu, rychlosti, použití

---

### KATEGORIE 2: SMART CONTRACTS

#### Otázka:
```
Jaké jsou nejčastější bezpečnostní chyby v smart contractech?
```

#### Co AI odpovídá:
- Reentrancy attack (The DAO hack)
- Integer overflow/underflow
- Front-running
- Access control issues
- Oracle manipulation

---

#### Otázka:
```
Analyzuj tento Solidity kód na bezpečnost:

pragma solidity ^0.8.0;

contract Vault {
    mapping(address => uint) public balances;
    
    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }
    
    function withdraw(uint amount) public {
        require(balances[msg.sender] >= amount);
        (bool success, ) = msg.sender.call{value: amount}("");
        require(success);
        balances[msg.sender] -= amount;
    }
}
```

#### Co AI by měla říci:
- ✅ Kontroluje zůstatek
- ✅ Používá call místo transfer
- ❌ **CEE-B3**: Reentrancy vulnerability! Updatuj stav PŘED send
- ❌ Chybí nonceReentrancyGuard
- 🔧 Oprava: Udělej checks-effects-interactions pattern

---

### KATEGORIE 3: DEFI PROTOKOLY

#### Otázka:
```
Jak funguje Uniswap a jaké má rizika?
```

#### Co AI odpovídá:
- Constant product formula (x*y=k)
- Liquidity providers a fees
- Slippage a price impact
- Impermanent loss
- Flash loan vulnerabilities

---

#### Otázka:
```
Chci si vložit likviditu na Uniswap v USDC/ETH páru. Jaká jsou rizika?
```

#### Co AI by měla říci:
- ⚠️ Nejsem finanční poradce
- Impermanent loss: pokud se ceny hodně změní
- Smart contract rizika: exploity v Uniswap
- Gas fees: mohou být vysoké
- Rug pull: pokud je to nový token, může být scam
- DYOR: dělej vlastní research

---

### KATEGORIE 4: TOKENOMIKA

#### Otázka:
```
Analyzuj tokenomiku tohoto projektu:
- Total supply: 1 bilion tokenů
- Vesting: 50% locked na 2 roky
- Team: 20% tokenů
- Community: 30% tokenů
```

#### Co AI odpovídá:
- Podíl jednotlivých skupin
- Risk: Team má velký podíl
- Vesting je dobrý (zabrání dump)
- Community podíl je v pořádku
- Cena by mohla padnout když skončí vesting

---

### KATEGORIE 5: TRANSAKCE & BLOCKCHAIN EXPLORER

#### Otázka:
```
Vysvětli mi co se v této transakci stalo:
0x1234567890abcdef...

(a co v ní vidím na Etherscan)
```

#### Co AI odpovídá:
- Kdo poslal (from)
- Kdo přijal (to)
- Co se stalo (function call)
- Kolik stálo na gasu
- Byl to úspěch nebo selhání
- Jak dlouho to trvalo

---

### KATEGORIE 6: KÓDOVÁNÍ & PYTHON

#### Otázka:
```
Napiš mi Python skript který bude sledovat cenu ETH
```

#### Co AI vrátí:
```python
import requests
import time

def get_eth_price():
    url = "https://api.coingecko.com/api/v3/simple/price"
    params = {"ids": "ethereum", "vs_currencies": "usd"}
    
    while True:
        try:
            response = requests.get(url, params=params)
            data = response.json()
            price = data["ethereum"]["usd"]
            print(f"ETH cena: ${price}")
            time.sleep(60)
        except Exception as e:
            print(f"Chyba: {e}")

if __name__ == "__main__":
    get_eth_price()
```

---

#### Otázka:
```
Napiš skript které se pokojí na smart contract na Ethereu
```

#### Co AI vrátí:
```python
from web3 import Web3

# Připoj se
w3 = Web3(Web3.HTTPProvider("https://eth-mainnet.g.alchemy.com/v2/YOUR_API_KEY"))

# Kontraktní address
contract_address = "0x..."

# Přečti hodnotu
contract = w3.eth.contract(address=contract_address, abi=ABI)
value = contract.functions.getValue().call()

print(f"Hodnota: {value}")
```

---

### KATEGORIE 7: BEZPEČNOST & BEST PRACTICES

#### Otázka:
```
Je bezpečné poslat moji seed phrase přes email někomu komu věřím?
```

#### Co AI odpovídá:
- **NE, absolutně ne!**
- Seed phrase = všechny peníze
- Email NENÍ šifrovaný
- Kdyby byla hacknutá email, všechno je ztraceno
- Nikdy ji komu neposílej

---

#### Otázka:
```
Jak si bezpečně uložit seed phrase?
```

#### Co AI odpovídá:
1. Papír: Napiš si offline, složit to do trezoru
2. Metal seed plate: Odolá ohni, vodě
3. Cold wallet hardware: Ledger, Trezor
4. NIKDY: Screenshot, cloud, email, chat

---

### KATEGORIE 8: MULTI-CHAIN TOPICS

#### Otázka:
```
Porovnej Ethereum, Solana a Bitcoin z hlediska:
1. Scalovatelelnosti
2. Decentralizace
3. Bezpečnosti
4. Nákladů
```

#### Co AI odpovídá:
- Ethereum: střední gas, vysoká bezpečnost, PoS
- Solana: nízké gas, vysoká rychlost, ale méně decentralizovaná
- Bitcoin: nejvíce decentralizovaný, nejpomalejší, nejvíce zaveřený

---

### KATEGORIE 9: DeFi STRATEGIE (EDUKAČNÍ)

#### Otázka:
```
Co je yield farming a jaké má rizika?
```

#### Co AI odpovídá:
- Deposit tokenů do protokolu za interest
- Smart contract rizika
- Impermanent loss (AMM likvidita)
- Depeg rizika
- Vysoké APY = vysoké riziko

---

### KATEGORIE 10: REGULACE & COMPLIANCE

#### Otázka:
```
Je legální koupit si Bitcoin v ČR?
```

#### Co AI odpovídá:
- ✅ Ano, je to legální
- Daně: musíš hlásit příjmy a prodeje
- KYC/AML: burzy vyžadují ověření
- Střet: záleží na situaci (daň z přidané hodnoty, spotřebitel?)
- Poraď si s daňovým poradcem!

---

## 🚀 POKROČILÉ USE CASES

### Use Case 1: Contract Audit Simulation

```
Tady je nový ERC-20 token. Audituji ho na bezpečnost:

[vlož kód]

Jaká vidíš rizika?
```

### Use Case 2: DeFi Yield Calculator

```
Mám 10 ETH. Chci ho vložit na Uniswap v páru ETH/USDC.
Pool fee je 0.3%, current APR je 15%.
Jaký výnos si mohu očekávat za měsíc (bez impermanent loss)?
```

### Use Case 3: Gas Optimization

```
Optimalizuj tento kód z hlediska gasu:
[vlož kód]
```

### Use Case 4: Cross-Chain Analysis

```
Jaké jsou rozdíly v implementaci ERC-20 tokenu na:
- Ethereum
- Polygon
- Arbitrum
- Optimism
```

---

## ⚠️ SITUACE KTERÁ NEBUDOU FUNGOVAT

❌ **To AI NEBUDE dělat:**

```
"Koupi si tento token, půjde na měsíc 100x!"
```
→ Odmítne, dá ti disclaimer

```
"Udělej mi trading bot který vydělá garantovaně"
```
→ Řekne že to není možné

```
"Jaký je nejlepší fork vytvořit?"
```
→ Vysvětlí co je to, ale nebude ti radít jak udělat scam

```
"Vymysli mi scheme jak okrást lidi"
```
→ Absolutně ne!

---

## 🎯 NEJLEPŠÍ PRAKTIKY

### Co dělat:
✅ Ptej se na **jak to funguje**  
✅ Ptej se na **bezpečnost**  
✅ Ptej se na **edukaci**  
✅ Ptej se na **kódování**  
✅ Ověřuj odpovědi na blockchain exploreru

### Co nedělat:
❌ Neočekávej finanční rady  
❌ Neočekávej predikce cen  
❌ Nepředpokládej že AI je vždy správná  
❌ Neposílej seed phrase  
❌ Nečiň rozhodnutí pouze na základě AI

---

## 📊 PŘÍKLAD REÁLNÉ PRÁCE

**Scénář**: Chceš zkontrolovat nový DeFi protokol

**Krok 1**: Ptej se na concept
```
Jak funguje Yearn Finance?
```

**Krok 2**: Ptej se na kód
```
Čti Yearn strategii a vysvětli mi ji
```

**Krok 3**: Ptej se na rizika
```
Jaká vidíš rizika v Yearn strategii?
```

**Krok 4**: Ptej se na integraci
```
Jak bych mohl vložit svými ETH do Yearn?
```

**Krok 5**: Ověřuj
```
Jdi na YearnFinance.com a zkontroluj sám
```

---

## 🎓 LEARNING PATH

1. **Den 1**: Porozumění blockchainů (Bitcoin vs Ethereum)
2. **Den 2**: Smart contracts (Solidity basics)
3. **Den 3**: DeFi protokoly (Uniswap, Aave)
4. **Den 4**: Bezpečnost (private keys, seed phrases)
5. **Den 5**: Kódování (Python + Web3.py)
6. **Den 6-7**: Praktický projekt

---

Každou otázku si vyzkoušej s AI! 🚀

Máš jakoukoli otázku? Ptej se promptu!
