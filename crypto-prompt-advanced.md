# 🔧 POKROČILÁ INTEGRACE - Pro Vývojáře

Pokročilý guide pro integraci promptu do custom systémů.

---

## 🏗️ ARCHITEKTURA

```
┌─────────────────────────────────────────┐
│     Vaše Aplikace / Bot                 │
└────────────────┬────────────────────────┘
                 │
                 ▼
         ┌──────────────┐
         │  LLM Handler │
         └───────┬──────┘
                 │
        ┌────────┼────────┐
        │        │        │
       ▼        ▼        ▼
    Gemini   OpenAI   Claude
    
└─ Všechny s CRYPTO-SYSTEM-PROMPT
```

---

## 🔌 IMPLEMENTACE V RŮZNÝCH JAZYCÍCH

### JAVASCRIPT / TypeScript

```typescript
import Anthropic from "@anthropic-ai/sdk";
import fs from "fs";

const SYSTEM_PROMPT = fs.readFileSync("crypto-system-prompt.txt", "utf-8");

const client = new Anthropic();

async function askCryptoAI(userMessage: string): Promise<string> {
  const response = await client.messages.create({
    model: "claude-3-5-sonnet-20241022",
    max_tokens: 2048,
    system: SYSTEM_PROMPT,
    messages: [
      {
        role: "user",
        content: userMessage,
      },
    ],
  });

  const textContent = response.content[0];
  if (textContent.type === "text") {
    return textContent.text;
  }
  throw new Error("Unexpected response type");
}

// Použití
(async () => {
  const response = await askCryptoAI("Vysvětli DeFi");
  console.log(response);
})();
```

**Instalace:**
```bash
npm install @anthropic-ai/sdk
```

---

### PYTHON (web3.py + AI)

```python
#!/usr/bin/env python3
"""
Advanced Crypto AI + Web3 Integration
"""

import os
from pathlib import Path
from anthropic import Anthropic

def load_system_prompt() -> str:
    """Načti crypto system prompt"""
    prompt_file = Path(__file__).parent / "crypto-system-prompt.txt"
    with open(prompt_file, "r", encoding="utf-8") as f:
        return f.read()

class CryptoAIAssistant:
    def __init__(self):
        self.client = Anthropic()
        self.system_prompt = load_system_prompt()
        self.conversation_history = []
    
    def chat(self, user_message: str) -> str:
        """Send message and get response"""
        self.conversation_history.append({
            "role": "user",
            "content": user_message
        })
        
        response = self.client.messages.create(
            model="claude-3-5-sonnet-20241022",
            max_tokens=2048,
            system=self.system_prompt,
            messages=self.conversation_history
        )
        
        assistant_message = response.content[0].text
        self.conversation_history.append({
            "role": "assistant",
            "content": assistant_message
        })
        
        return assistant_message
    
    def analyze_contract(self, contract_code: str) -> str:
        """Analyze Solidity contract"""
        message = f"""Prosím analyzuj tento smart contract na bezpečnost:

```solidity
{contract_code}
```

Mě zajímá:
1. Co kontakt dělá?
2. Jaké vidíš bezpečnostní chyby?
3. Jaké jsou doporučení na opravu?
"""
        return self.chat(message)
    
    def explain_transaction(self, tx_hash: str) -> str:
        """Explain Ethereum transaction"""
        message = f"""Vysvětli mi co se v transakci {tx_hash} stalo.
Pokud ji znáš, vysvětli detaily. Pokud ne, řekni jak by se dala analyzovat."""
        return self.chat(message)
    
    def defi_analysis(self, protocol_name: str, query: str) -> str:
        """Analyze DeFi protocol"""
        message = f"Ohledně {protocol_name}: {query}"
        return self.chat(message)

# Příklad použití
if __name__ == "__main__":
    ai = CryptoAIAssistant()
    
    # Základní chat
    response = ai.chat("Jak fungují smart contracts?")
    print(response)
    print("\n" + "="*50 + "\n")
    
    # Follow-up (má context)
    response = ai.chat("A jaké jsou nejčastější chyby?")
    print(response)
```

**Instalace:**
```bash
pip install anthropic
export ANTHROPIC_API_KEY="your-key-here"
python script.py
```

---

### RUST (Pro Solana / Rust Smart Contracts)

```rust
use anthropic::client::Client;
use std::fs;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let api_key = std::env::var("ANTHROPIC_API_KEY")?;
    let client = Client::new(api_key);
    
    // Načti system prompt
    let system_prompt = fs::read_to_string("crypto-system-prompt.txt")?;
    
    // Vytvoř message
    let message = client
        .messages()
        .create(
            "claude-3-5-sonnet-20241022",
            vec![],
            "Vysvětli Solana blockchain".to_string(),
        )
        .system(system_prompt)
        .await?;
    
    println!("{}", message.content);
    
    Ok(())
}
```

---

## 🔐 ENVIRONMENT VARIABLES

```bash
# .env file
ANTHROPIC_API_KEY=sk-ant-...
OPENAI_API_KEY=sk-...
GEMINI_API_KEY=...

# Či environment variables
export ANTHROPIC_API_KEY="sk-ant-..."
```

---

## 📡 API INTEGRATION PATTERNS

### Pattern 1: Chat Interface

```python
def create_chat_interface():
    """Interactive chat s crypto AI"""
    ai = CryptoAIAssistant()
    
    print("🤖 Crypto AI Assistant - Chat Mode")
    print("Type 'exit' to quit\n")
    
    while True:
        user_input = input("You: ").strip()
        if user_input.lower() == "exit":
            break
        
        response = ai.chat(user_input)
        print(f"\nAI: {response}\n")
```

### Pattern 2: Batch Analysis

```python
def batch_analyze_contracts(contract_list: list) -> dict:
    """Analyze multiple contracts"""
    ai = CryptoAIAssistant()
    results = {}
    
    for contract_name, contract_code in contract_list.items():
        analysis = ai.analyze_contract(contract_code)
        results[contract_name] = analysis
    
    return results
```

### Pattern 3: Scheduled Tasks

```python
import schedule
import time

def periodic_crypto_updates():
    """Spouštěj analyzu pravidelně"""
    ai = CryptoAIAssistant()
    
    schedule.every().day.at("10:00").do(
        lambda: ai.chat("Jaké jsou dnes novinky v DeFi?")
    )
    
    while True:
        schedule.run_pending()
        time.sleep(60)
```

### Pattern 4: Web Server

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()
ai = CryptoAIAssistant()

class CryptoQuery(BaseModel):
    question: str

@app.post("/api/crypto")
async def crypto_endpoint(query: CryptoQuery):
    response = ai.chat(query.question)
    return {"response": response}

# Spusť: uvicorn script:app --reload
# Uso: curl -X POST http://localhost:8000/api/crypto -d '{"question":"Co je DeFi?"}'
```

---

## 🎯 ADVANCED FEATURES

### Feature 1: Memory / Context Management

```python
class SmartCryptoAI:
    def __init__(self, context_size: int = 10):
        self.context_size = context_size
        self.conversation_history = []
    
    def prune_history(self):
        """Keep only recent messages"""
        if len(self.conversation_history) > self.context_size:
            self.conversation_history = self.conversation_history[-self.context_size:]
```

### Feature 2: Response Caching

```python
from functools import lru_cache

class CachedCryptoAI(CryptoAIAssistant):
    def __init__(self):
        super().__init__()
        self._response_cache = {}
    
    @lru_cache(maxsize=100)
    def cached_chat(self, user_message: str) -> str:
        """Chat with caching"""
        cache_key = hash(user_message)
        
        if cache_key in self._response_cache:
            return self._response_cache[cache_key]
        
        response = self.chat(user_message)
        self._response_cache[cache_key] = response
        return response
```

### Feature 3: Error Handling & Retry

```python
import asyncio
from tenacity import retry, stop_after_attempt, wait_exponential

class RobustCryptoAI(CryptoAIAssistant):
    @retry(
        stop=stop_after_attempt(3),
        wait=wait_exponential(multiplier=1, min=2, max=10)
    )
    async def chat_with_retry(self, user_message: str) -> str:
        """Chat s automatickým retry"""
        try:
            return self.chat(user_message)
        except Exception as e:
            print(f"Error: {e}, retrying...")
            raise
```

### Feature 4: Token Usage Tracking

```python
class MonitoredCryptoAI(CryptoAIAssistant):
    def __init__(self):
        super().__init__()
        self.total_input_tokens = 0
        self.total_output_tokens = 0
    
    def chat(self, user_message: str) -> str:
        response = super().chat(user_message)
        
        # Track usage (varies by API)
        # self.total_input_tokens += response.usage.input_tokens
        # self.total_output_tokens += response.usage.output_tokens
        
        return response
    
    def print_usage(self):
        print(f"Total tokens used:")
        print(f"  Input: {self.total_input_tokens}")
        print(f"  Output: {self.total_output_tokens}")
```

---

## 🧪 TESTING

```python
import pytest
from unittest.mock import patch, MagicMock

def test_crypto_ai_initialization():
    ai = CryptoAIAssistant()
    assert ai.system_prompt is not None
    assert "blockchain" in ai.system_prompt.lower()

def test_contract_analysis():
    ai = CryptoAIAssistant()
    contract = "pragma solidity ^0.8.0; contract Test {}"
    
    response = ai.analyze_contract(contract)
    assert response is not None
    assert len(response) > 0

@patch('anthropic.Anthropic')
def test_api_call(mock_client):
    ai = CryptoAIAssistant()
    ai.client = mock_client
    
    # Test that API is called correctly
    assert ai.client is not None
```

**Spusť testy:**
```bash
pytest test_crypto_ai.py -v
```

---

## 📊 MONITORING & LOGGING

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)

logger = logging.getLogger(__name__)

class LoggedCryptoAI(CryptoAIAssistant):
    def chat(self, user_message: str) -> str:
        logger.info(f"User message: {user_message[:50]}...")
        
        response = super().chat(user_message)
        
        logger.info(f"AI response length: {len(response)}")
        return response
```

---

## 🚀 DEPLOYMENT

### Docker

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "crypto_ai_bot.py"]
```

```bash
# Build
docker build -t crypto-ai .

# Run
docker run -e ANTHROPIC_API_KEY="..." crypto-ai
```

### Environment Setup

```bash
# requirements.txt
anthropic>=0.7.0
web3>=6.0.0
python-dotenv>=0.20.0
fastapi>=0.100.0
uvicorn>=0.23.0
tenacity>=8.2.0
```

---

## 📚 BEST PRACTICES

✅ **DO:**
- Load system prompt once at startup
- Cache responses when possible
- Add error handling & retry logic
- Log all interactions
- Use environment variables for secrets
- Add rate limiting
- Monitor token usage
- Test with mock data first

❌ **DON'T:**
- Hardcode API keys
- Send sensitive data in logs
- Create new AI instance per message
- Ignore error responses
- Forget to validate user input
- Store API keys in version control

---

## 🔗 USEFUL RESOURCES

- Anthropic Python SDK: https://github.com/anthropics/anthropic-sdk-python
- OpenAI Python SDK: https://github.com/openai/openai-python
- Web3.py: https://docs.web3py.org/
- Ethers.py: https://docs.ethers.org/v6/
- FastAPI: https://fastapi.tiangolo.com/

---

**Potřebuješ pomoc s pokročilou integrací?** Kontaktuj vývojáře nebo ptej se promptu!
