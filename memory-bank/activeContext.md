# Active Context for Passivbot

## Current Work Focus

The current focus is on understanding and documenting the Passivbot project structure, architecture, and functionality through the creation of a comprehensive memory bank. This serves as a foundation for future development, maintenance, and enhancement work.

### Primary Areas of Focus

1. **Project Documentation**
   - Creating the memory bank structure
   - Documenting core components, architecture, and design patterns
   - Establishing a baseline understanding of the system

2. **System Architecture Understanding**
   - Analyzing the relationship between Python and Rust components
   - Understanding the exchange adapter system
   - Mapping the optimization and backtesting architecture

3. **Configuration System**
   - Understanding how different parameters affect the trading strategy
   - Documenting configuration options and their effects
   - Analyzing the relationship between configuration and performance

4. **Installation and Setup Process**
   - Documenting the complete installation process
   - Identifying required dependencies and their versions
   - Ensuring smooth setup across different environments

## Recent Changes

The following major setup activities and documentation have been established:

1. **Environment Setup and Installation**
   - Python 3.11+ and Rust 1.86+ confirmed as working environment
   - Virtual environment created and activated
   - All critical dependencies installed and tested
   - Rust components verified as properly built
   - Basic bot execution validated with example configuration
   - API keys structure confirmed (example keys not valid for actual trading)

2. **Memory Bank Creation**
   - Project brief has been preserved from existing documentation
   - Product context has been created based on project understanding
   - System patterns have been documented to capture architecture
   - Technical context has been established to document technology stack
   - Active context (this document) has been created and updated with installation instructions
   - Progress tracking established

3. **Knowledge Organization**
   - Core trading strategies have been identified and documented
   - Key components have been mapped in system architecture diagrams
   - Technical stack and dependencies have been cataloged

## Installation Guide

### Prerequisites

1. **Python 3.8+** (tested with Python 3.11.3)

   ```bash
   python3 --version
   ```

2. **Rust** (tested with Rust 1.86.0)

   ```bash
   rustc --version
   ```

   - If not installed, follow instructions at [https://www.rust-lang.org/tools/install](https://www.rust-lang.org/tools/install)

### Step-by-Step Installation Process

1. **Clone the Repository** (if not already done)

   ```bash
   git clone https://github.com/enarjord/passivbot.git
   cd passivbot
   ```

2. **Create and Activate Virtual Environment**

   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install Core Dependencies First**

   ```bash
   pip install sortedcontainers python-dateutil maturin
   ```

4. **Install Data Handling and Visualization Dependencies**

   ```bash
   pip install pandas matplotlib numpy
   ```

5. **Install Exchange API Dependencies (Exact Version Required)**

   ```bash
   pip install ccxt==4.4.77
   ```

6. **Install Remaining Dependencies**

   ```bash
   pip install -r requirements.txt
   ```

   - Note: Some dependencies may fail to install, but the bot can still function with the core dependencies

7. **Build Rust Extensions** (if not already built)

   ```bash
   cd passivbot-rust
   maturin develop --release
   cd ..
   ```

8. **Set Up API Keys**
   - Copy the example file:

     ```bash
     cp api-keys.json.example api-keys.json
     ```

   - Edit api-keys.json with your exchange API keys and secrets

9. **Verify Installation**
   - Check that the Rust extension can be imported:

     ```bash
     python -c "import passivbot_rust; print('Import successful')"
     ```

   - Check that the bot can start (it will attempt to authenticate with the exchange):

     ```bash
     python3 src/main.py --help
     ```

### Running the Bot

1. **Run with Default Settings**

   ```bash
   python3 src/main.py -u account_name_from_api-keys.json
   ```

2. **Run with Specific Configuration**

   ```bash
   python3 src/main.py configs/examples/btc_long.json -u account_name_from_api-keys.json
   ```

3. **Common Example Configurations**
   - BTC Long only: `configs/examples/btc_long.json`
   - Multiple coins (BTC, ETH, XRP, SOL, ADA): `configs/examples/BTC_ETH_XRP_SOL_ADA_long.json`
   - All approved coins: `configs/examples/all_approved.json`

### Troubleshooting

1. **Missing Dependencies**
   - If you encounter "ModuleNotFoundError", install the specific missing module:

     ```bash
     pip install module_name
     ```

2. **Rust Component Issues**
   - Rebuild the Rust components:

     ```bash
     cd passivbot-rust
     maturin develop --release
     cd ..
     ```

3. **Exchange API Issues**
   - Verify your API keys are correct in api-keys.json
   - Check that you're using the correct account name with the -u flag
   - Ensure ccxt version is exactly 4.4.77 as required:

     ```bash
     pip install ccxt==4.4.77
     ```

## Next Steps

The following immediate next steps are planned:

1. **Complete Memory Bank Setup**
   - Finalize remaining memory bank documents
   - Create .clinerules file for project-specific patterns
   - Establish progress tracking baseline

2. **Deep Dive into Core Components**
   - Analyze the main trading algorithm implementation
   - Examine the optimization system in detail
   - Understand the exchange integration mechanisms
   - Document the backtesting and simulation system

3. **Configuration Analysis**
   - Examine example configurations to understand parameter ranges
   - Document the effect of different parameters on strategy behavior
   - Create reference documentation for configuration options

4. **Potential Enhancement Areas**
   - Identify opportunities for improved documentation
   - Note potential areas for performance optimization
   - Consider additions to visualization and analysis tools

## Active Decisions and Considerations

### Architectural Decisions

1. **Python/Rust Division of Responsibility**
   - Python handles coordination, user interface, and exchange communication
   - Rust handles performance-critical calculations and backtesting
   - This hybrid approach balances flexibility with performance
   - Future work should maintain this separation of concerns

2. **Exchange Adapter Design**
   - Exchange-specific code is isolated in adapter modules
   - Core logic remains exchange-agnostic
   - New exchange integrations should follow existing adapter pattern
   - Consider refactoring common adapter functionality if adding new exchanges

3. **Configuration System**
   - JSON-based configuration provides flexibility
   - Parameters are validated and normalized at runtime
   - Future enhancements should maintain backward compatibility
   - Consider adding schema validation for configuration files

### Strategic Considerations

1. **Performance Optimization**
   - Backtesting speed is critical for optimization effectiveness
   - Continue to move performance-critical code to Rust where appropriate
   - Consider parallel processing for optimization workloads
   - Profile and optimize hot paths in both Python and Rust code

2. **Risk Management**
   - The unstucking mechanism is a key risk management feature
   - Balance between aggressive trading and capital preservation
   - Consider additional risk controls for extreme market conditions
   - Evaluate position sizing algorithms for effectiveness

3. **User Experience**
   - Balance between flexibility and ease of use
   - Focus on clear documentation and examples
   - Consider visualization improvements for strategy understanding
   - Evaluate logging and monitoring capabilities for live trading

### Current Questions and Unknowns

1. **Exchange-Specific Behavior**
   - How do different exchanges behave during extreme market conditions?
   - Are there significant latency differences between exchanges?
   - How do exchange fees affect strategy profitability?

2. **Strategy Parameter Sensitivity**
   - Which parameters have the largest impact on performance?
   - Are there parameter combinations that should be avoided?
   - How does parameter optimization transfer between market conditions?

3. **Long-Term Performance**
   - How does the strategy perform across different market regimes?
   - What is the expected drawdown during adverse conditions?
   - How frequently should parameters be re-optimized?

These active considerations will guide future exploration and development efforts as we work with the Passivbot system.
