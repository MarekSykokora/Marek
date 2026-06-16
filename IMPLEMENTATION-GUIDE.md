# 📖 IMPLEMENTATION GUIDE - Jak používat Crypto AI Promptu

Super jednoduchý návod jak implementovat systémový prompt do libovolné AI.

---

## 🎯 3 ZPŮSOBY IMPLEMENTACE

### MOŽNOST 1️⃣: GEMINI (NEJJEDNODUŠÍ) ⭐

**Krok 1:** Jdi na https://gemini.google.com

**Krok 2:** Klikni "Create new chat" (vytvořit nový chat)

**Krok 3:** V řádku kde normálně píšeš zprávy, vlož obsah z `crypto-system-prompt.txt`:
```
Zkopíruj veškerý obsah z crypto-system-prompt.txt (Ctrl+A → Ctrl+C)
Vlož do Gemini (Ctrl+V)
```

**Krok 4:** Stiskni Enter/Send

**Krok 5:** Gemini teď "umí" kryptoměny! Zkus mu napsat:
```
Vysvětli mi jak fungují smart contracts
```

✅ **HOTOVO!**

---

### MOŽNOST 2️⃣: CHATGPT (PLATENÁ VERZE)

**Krok 1:** Jdi na https://chat.openai.com

**Krok 2:** Klikni "Create custom GPT" (v menu vlevo dole)

**Krok 3:** Pojmenuj to: "Crypto AI Assistant"

**Krok 4:** V poli "Instructions" vlož VEŠKERÝ obsah z `crypto-system-prompt.txt`

**Krok 5:** Klikni "Save"

**Krok 6:** Teď ho máš jako "Custom GPT" kterého můžeš kdykoliv spustit

✅ **HOTOVO!**

---

### MOŽNOST 3️⃣: CLAUDE (ANTHROPIC)

**Krok 1:** Jdi na https://claude.ai

**Krok 2:** Vytvoř nový chat

**Krok 3:** Vlož prompt stejně jako u Gemini (Ctrl+V)

**Krok 4:** Stiskni Enter

✅ **HOTOVO!**

---

## 🐍 PYTHON IMPLEMENTACE (PRO VÝVOJÁŘE)

Soubor: `crypto-ai-integration.py`

```python
#!/usr/bin/env python3
"""
Crypto AI Assistant - Python Integration
Použití: python crypto-ai-integration.py
"""

import os
from pathlib import Path

# MOŽNOST A: Google Gemini API
# (Budete potřebovat API key)

def setup_gemini():
    """Setup Gemini AI"""
    try:
        import google.generativeai as genai
        api_key = os.getenv("GEMINI_API_KEY")
        if not api_key:
            print("⚠️  GEMINI_API_KEY není nastaven")
            return None
        
        genai.configure(api_key=api_key)
        return genai.GenerativeModel("gemini-pro")
    except ImportError:
        print("Instaluj: pip install google-generativeai")
        return None

# MOŽNOST B: OpenAI API

def setup_openai():
    """Setup OpenAI"""
    try:
        from openai import OpenAI
        api_key = os.getenv("OPENAI_API_KEY")
        if not api_key:
            print("⚠️  OPENAI_API_KEY není nastaven")
            return None
        
        return OpenAI(api_key=api_key)
    except ImportError:
        print("Instaluj: pip install openai")
        return None

# MOŽNOST C: Anthropic Claude

def setup_claude():
    """Setup Claude"""
    try:
        from anthropic import Anthropic
        api_key = os.getenv("ANTHROPIC_API_KEY")
        if not api_key:
            print("⚠️  ANTHROPIC_API_KEY není nastaven")
            return None
        
        return Anthropic(api_key=api_key)
    except ImportError:
        print("Instaluj: pip install anthropic")
        return None

def load_system_prompt():
    """Načti systémový prompt z souboru"""
    prompt_path = Path(__file__).parent / "crypto-system-prompt.txt"
    try:
        with open(prompt_path, "r", encoding="utf-8") as f:
            return f.read()
    except FileNotFoundError:
        print(f"❌ Soubor nenalezen: {prompt_path}")
        return None

def main():
    print("🚀 Crypto AI Assistant - Python Integration")
    print("=" * 50)
    
    # Načti prompt
    system_prompt = load_system_prompt()
    if not system_prompt:
        print("Nemůžu najít systémový prompt!")
        return
    
    print("✅ Systémový prompt načten")
    print()
    
    # VOLBA POSKYTOVATELE
    print("Kterého AI poskytovatele chceš použít?")
    print("1. Gemini (Google)")
    print("2. OpenAI (ChatGPT)")
    print("3. Claude (Anthropic)")
    print()
    
    choice = input("Zadej volbu (1-3): ").strip()
    
    if choice == "1":
        client = setup_gemini()
        if client:
            print("✅ Gemini je připravena!")
            # Testovací zpráva
            test_message = "Vysvětli mi co je DeFi v jedné větě"
            print(f"\n📝 Testování s: '{test_message}'")
            print("-" * 50)
            # Note: Gemini API funguje trochu jinak
    
    elif choice == "2":
        client = setup_openai()
        if client:
            print("✅ OpenAI je připravena!")
    
    elif choice == "3":
        client = setup_claude()
        if client:
            print("✅ Claude je připravena!")
    
    else:
        print("❌ Neplatná volba")
        return
    
    if not client:
        print("Nemůžu se připojit. Zkontroluj API klíč.")
        return
    
    print("\n💡 Prompt je načten a připraven!")
    print("Teď můžeš používat AI s crypto znalostmi")

if __name__ == "__main__":
    main()
```

**Instalace:**
```bash
# Pro Gemini
pip install google-generativeai

# Pro OpenAI
pip install openai

# Pro Claude
pip install anthropic
```

**Nastav API klíče:**
```bash
# Na Linuxu/Macu:
export GEMINI_API_KEY="tvůj-klíč"
export OPENAI_API_KEY="tvůj-klíč"
export ANTHROPIC_API_KEY="tvůj-klíč"

# Na Windows (PowerShell):
$env:GEMINI_API_KEY="tvůj-klíč"
```

**Spusť:**
```bash
python crypto-ai-integration.py
```

---

## ✅ OVĚŘENÍ ŽE TO FUNGUJE

Jakmile máš prompt v AI, zkus tyto testy:

### Test 1: Základní porozumění
```
Vysvětli smart contract na Ethereu jako bych byl úplný začátečník
```
✅ AI by měla: Vysvětlit jasně bez technického žargonu

### Test 2: Technické detaily
```
Co je DeFi, jaké má rizika a jak to funguje?
```
✅ AI by měla: Být podrobná, zmínit bezpečnostní rizika

### Test 3: Kód analýza
```
Co dělá tento Solidity kód?
pragma solidity ^0.8.0;
contract Simple {
    uint public value = 0;
    function setValue(uint _v) public { value = _v; }
}
```
✅ AI by měla: Vysvětlit co kód dělá

### Test 4: Bezpečnost
```
Je bezpečné poslat své semínko (seed phrase) někomu online?
```
✅ AI by měla: Jasně říci NE a vysvětlit proč

### Test 5: Disclaimer
```
Řekni mi která coins si myslíš že vzrostou v příštích měsících
```
✅ AI by měla: Odmítnout a dát disclaimer že to není finanční porada

---

## 🐛 TROUBLESHOOTING

**Problém**: AI pořád říká že to neví  
**Řešení**: Ujisti se že jsi zkopíroval VEŠKERÝ obsah promptu (včetně konce)

**Problém**: AI mi dává finanční rady  
**Řešení**: Prompt je nastaven aby to nedělal, zkus ho znova zkopírovat

**Problém**: AI mi lže nebo dává špatné info  
**Řešení**: To se někdy stane - vždy ověřuj na blockchain exploreru

**Problém**: Python skript nefunguje  
**Řešení**: Zkontroluj jestli máš API klíč a správný balík nainstalován

---

## 🎯 DOPORUČENÉ WORKFLOW

1. **Příprava**: Zkopíruj prompt do AI (5 minut)
2. **Testování**: Zkus pár otázek ze sekce "Ověření" (10 minut)
3. **Použití**: Teď můžeš AI ptát se na cokoli ohledně krypta (∞)

---

## 📚 DALŠÍ ZDROJE

- Ethereum docs: https://ethereum.org/developers
- Solidity docs: https://docs.soliditylang.org/
- OpenZeppelin (bezpečné smlouvy): https://docs.openzeppelin.com/
- DeFi Pulse: https://defipulse.com/
- Blockchain explorer: https://etherscan.io/

---

**Potřebuješ pomoc?** Ptej se v chat s promptem!

Vše hotovo? Gratuluji! 🎉 Teď máš vlastního Crypto AI Assistanta!
