# Customer Support Agent

A multi-agent AI customer support system built with OpenAI Agents, featuring voice interaction, intelligent triage, and specialized support agents for technical, billing, order, and account management.

## Features

- 🎤 **Voice Interaction**: Real-time voice input and text-to-speech responses
- 🤖 **Intelligent Triage**: Automatic routing to specialized support agents
- 🛠️ **Technical Support**: Product diagnostics, troubleshooting, and engineering escalation
- 💰 **Billing Support**: Payment history, refunds, payment method updates
- 📦 **Order Management**: Order tracking, returns, shipping, and delivery
- 👤 **Account Management**: Password resets, security settings, profile updates
- 🛡️ **Input/Output Guardrails**: Content filtering and safety checks
- 💾 **Session Memory**: Persistent conversation history via SQLite
- 🎨 **Modern UI**: Streamlit-based web interface

## Architecture

The system uses a multi-agent architecture with intelligent handoff:

~~~text
┌─────────────────┐
│  Triage Agent   │ ← Entry point, routes to specialists
└────────┬────────┘
         │
    ┌────┴────┬─────────┬────────────┐
    │         │         │            │
    ▼         ▼         ▼            ▼
┌────────┐ ┌──────┐ ┌────────┐  ┌─────────┐
│Technical│ │Billing││ Order  │  │ Account │
│  Agent  │ │ Agent│ │ Agent  │  │  Agent  │
└────────┘ └──────┘ └────────┘  └─────────┘
~~~

### Agents

1. **Triage Agent** (`my_agents/triage_agent.py`)

    - Classifies customer issues into four categories
    - Routes to appropriate specialist
    - Handles multi-issue scenarios

2. **Technical Support Agent** (`my_agents/technical_agent.py`)

    - Product diagnostics and troubleshooting
    - Step-by-step problem resolution
    - Engineering escalation for complex issues

3. **Billing Support Agent** (`my_agents/billing_agent.py`)

    - Payment history and refunds
    - Subscription management
    - Payment method updates

4. **Order Management Agent** (`my_agents/order_agent.py`)

    - Order tracking and status
    - Returns and exchanges
    - Shipping and delivery management

5. **Account Management Agent** (`my_agents/account_agent.py`)

    - Password resets and authentication
    - Security settings (2FA)
    - Profile and email updates

## Installation

### Prerequisites

- Python 3.13+
- UV package manager (or compatible package manager)

### Setup

1. Clone the repository:

   ```bash
   git clone <repository-url>
   cd customer-support-agent
   ```

2. Install dependencies:

   ```bash
   uv sync
   ```

3. Create a `.env` file in the root directory:

   ```env
   OPENAI_API_KEY=your_openai_api_key_here
   ```

4. Run the application:

   ```bash
   streamlit run main.py
   ```

   The application will be available at `http://localhost:8501`

## Usage

1. **Start Recording**: Click the microphone button to record your message
2. **Speak Your Issue**: Describe your problem or question
3. **Receive Support**: The AI agent will understand your issue, route you to the right specialist, and provide solutions
4. **Follow Up**: Continue the conversation as needed

### Example Interactions

**Technical Issue:**

- "My app keeps crashing when I try to export data"

**Billing Question:**

- "I was charged twice this month, can I get a refund?"

**Order Inquiry:**

- "Where is my order #12345?"

**Account Help:**

- "I forgot my password, can you help me reset it?"

## Project Structure

~~~text
customer-support-agent/
├── main.py                    # Streamlit application entry point
├── models.py                  # Pydantic models for data structures
├── tools.py                   # Agent tool functions
├── workflow.py                # Voice workflow implementation
├── output_guardrails.py      # Output validation and guardrails
├── my_agents/
│   ├── triage_agent.py        # Intelligent routing agent
│   ├── technical_agent.py     # Technical support specialist
│   ├── billing_agent.py       # Billing support specialist
│   ├── order_agent.py         # Order management specialist
│   └── account_agent.py       # Account management specialist
├── customer-support-memory.db # SQLite session storage
├── pyproject.toml             # Project dependencies
└── uv.lock                    # Locked dependencies
~~~

## Technologies

- **OpenAI Agents**: Multi-agent AI framework
- **Streamlit**: Web application framework
- **Voice Pipeline**: Real-time voice input and synthesis
- **SQLite**: Persistent session storage
- **Pydantic**: Data validation and models
- **Python-dotenv**: Environment variable management

## Security & Guardrails

### Input Guardrails

- Validates user input is on-topic
- Filters out unrelated requests
- Ensures customer service focus

### Output Guardrails

- Technical agents cannot access billing/account/order data
- Prevents information leaks between agent specializations
- Ensures appropriate responses for each agent type

## Customization

### Adding New Agents

1. Create agent file in `my_agents/`
2. Import and configure in `triage_agent.py`
3. Add appropriate tools from `tools.py`

### Adding New Tools

Add function tools in `tools.py` following the existing pattern:

~~~python
@function_tool
def my_new_tool(context: UserAccountContext, param: str) -> str:
    """Tool description."""
    # Implementation
    return result
~~~

### Customer Tiers

The system supports different customer tiers:

- `basic`: Standard support
- `premium`: Enhanced features and priority
- `enterprise`: Highest priority and special handling

Modify `UserAccountContext` in `main.py` to change customer tier.

## Development

### Running Tests

~~~bash
# Add tests as needed
pytest tests/
~~~

### Linting

~~~bash
ruff check .
~~~

## License

License information not specified. Please check with the project maintainers for licensing details.

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Submit a pull request

## Support

For issues and questions, please open an issue on the repository.

## Acknowledgments

Built with OpenAI Agents framework for intelligent multi-agent orchestration.
