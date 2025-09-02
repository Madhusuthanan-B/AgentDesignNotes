# Encrypted Reasoning Continuity Test - Example 2

This example demonstrates how encrypted reasoning is passed between requests in OpenAI's Response API to maintain reasoning continuity across multiple interactions.

## Test Scenario
A number guessing game where the model uses binary search strategy and must adapt to new constraints while maintaining its previous reasoning approach.

## Files Overview

### 1. `1_response.json`
**Initial Request**: "I'm thinking of a number between 1 and 100. Can you guess what it is? Give me your reasoning for your guess."

**Model's Response**: 
- **Guess**: 50
- **Strategy**: Binary search approach using the midpoint
- **Reasoning**: "With no prior hints, the midpoint of 1–100 (which is 50) is the best first move in a classic 'guess the number' strategy"
- **Logic**: Maximizes information gained from the next hint by splitting the range evenly

**Key Elements**:
- `reasoning.effort`: "medium"
- `encrypted_content`: Contains the model's internal reasoning process
- `reasoning_tokens`: 256 (substantial reasoning activity)

### 2. `3_followup_request.json`
**Follow-up Constraints**: 
- "The number is higher than 50"
- "The number is divisible by 7"
- "Can you guess again using both your previous reasoning and this new information?"

**Request Structure**:
```json
{
    "input": [
        {"role": "user", "content": "Original question..."},
        {
            "id": "rs_68b7240ff64c8190bf1c09c60abd43ec0d9888216684fdef",
            "type": "reasoning",
            "encrypted_content": "gAAAAABotyQV...",
            "summary": []
        },
        {
            "id": "msg_68b72413648881908057e764caecfa570d9888216684fdef",
            "type": "message",
            "status": "completed",
            "content": [{"type": "output_text", "text": "I'll guess 50..."}],
            "role": "assistant"
        },
        {"role": "user", "content": "Follow-up with constraints..."}
    ],
    "include": ["reasoning.encrypted_content"]
}
```

### 3. `4_followup_response.json`
**Model's Adapted Response**:
- **Applied Constraints**: 
  - Range narrowed to 51-100 (higher than 50)
  - Found multiples of 7: 56, 63, 70, 77, 84, 91, 98
- **Continued Strategy**: Used same binary search logic
- **New Guess**: 77 (middle of the 7 candidates)
- **Reasoning**: "To split these seven candidates as evenly as possible, I'll pick the middle one: 77"

**Evidence of Reasoning Continuity**:
- Maintained binary search approach from first response
- Applied new constraints systematically
- Explained strategic reasoning consistent with original approach
- `reasoning_tokens`: 192 (continued active reasoning)

## Key Insights

### ✅ Successful Reasoning Continuity
1. **Strategy Consistency**: Model continued using divide-and-conquer approach
2. **Constraint Integration**: Successfully combined previous reasoning with new information
3. **Optimal Decision Making**: Chose 77 to maximize information gain from next response
4. **Explicit Reference**: Mentioned splitting candidates "as evenly as possible" - same logic as original 50 guess

### 🔧 Technical Implementation
- **Encrypted Content**: Previous reasoning passed as `encrypted_content` in input array
- **Complete Context**: Full conversation history including reasoning and message objects
- **Stateless Operation**: Works with `store: false` for stateless reasoning continuity
- **Token Efficiency**: Reasoning tokens decreased from 256 to 192, showing efficient reuse

### 📊 Performance Metrics
- **First Response**: 256 reasoning tokens, 380 total output tokens
- **Follow-up Response**: 192 reasoning tokens, 299 total output tokens
- **Context Preservation**: Successfully maintained strategic approach across requests

## Conclusion
This example proves that OpenAI's encrypted reasoning system successfully maintains reasoning continuity across stateless requests. The model effectively:
- Preserved its binary search strategy
- Integrated new constraints logically
- Made optimal decisions based on combined information
- Demonstrated clear reasoning progression

The encrypted reasoning mechanism enables sophisticated multi-turn reasoning without requiring server-side conversation storage.
