# Technical Context for Passivbot

## Technologies Used

### Core Languages

1. **Python (≥ 3.8)**
   - Primary language for high-level logic, API interactions, and user interface
   - Facilitates rapid development and ecosystem integration
   - Provides extensive libraries for data analysis and visualization

2. **Rust**
   - Used for performance-critical operations
   - Implements CPU-intensive backtesting algorithms
   - Provides memory safety with near-native performance
   - Compiled to platform-specific binary libraries

### Key Libraries and Frameworks

#### Python Libraries

1. **NumPy/Pandas**
   - Core data processing and numerical computations
   - Time series manipulation for price data
   - Statistical analysis for performance metrics

2. **ccxt**
   - Cryptocurrency exchange integration
   - Unified API for market data and trading
   - Handles exchange-specific quirks

3. **Numba**
   - Just-in-time compilation for performance-critical Python code
   - Accelerates numerical functions with @njit decoration
   - Enables near-C performance for pure Python algorithms

4. **websocket-client**
   - Real-time data streaming from exchanges
   - Event-driven architecture for price updates
   - Low-latency market data reception

5. **matplotlib/mplfinance**
   - Visualization of backtesting results
   - Candlestick charts and technical indicators
   - Performance metrics plotting

6. **hjson**
   - Human-readable JSON configuration files
   - More flexible syntax than standard JSON
   - Used for exchange-specific configuration

7. **Jupyter**
   - Interactive analysis and visualization
   - Development and debugging environment
   - Documentation of analysis processes

#### Rust Libraries

1. **Maturin**
   - Building and packaging Rust code as Python extensions
   - Creates platform-specific wheels for distribution
   - Manages Python/Rust interface

2. **PyO3**
   - Rust bindings for Python
   - Exposes Rust functions to Python code
   - Handles memory management between languages

3. **Serde**
   - Serialization/deserialization framework
   - Converts between Rust data structures and JSON
   - Efficiently handles configuration and data

### Infrastructure

1. **Docker**
   - Containerized deployment
   - Consistent runtime environment
   - Simplified dependency management

2. **ReadTheDocs**
   - Documentation hosting
   - Version-controlled documentation
   - Integration with GitHub repository

3. **GitHub**
   - Version control
   - Issue tracking
   - Release management

## Development Setup

### Local Development Environment

1. **Prerequisites**
   - Python 3.8 or newer
   - Rust toolchain (installed via rustup)
   - Virtual environment (venv or conda)
   - Git for version control

2. **Setup Process**
   ```bash
   # Clone repository
   git clone https://github.com/enarjord/passivbot.git
   cd passivbot
   
   # Create and activate virtual environment
   python3 -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   
   # Install dependencies
   pip install -r requirements.txt
   
   # Build Rust extensions (optional, will be built automatically on first run)
   cd passivbot-rust
   maturin develop --release
   cd ..
   ```

3. **Configuration**
   - Copy api-keys.json.example to api-keys.json
   - Add exchange API credentials
   - Create or modify configuration files in configs/

### Testing

1. **Backtesting**
   - Testing strategies on historical data
   - Automated performance metric calculation
   - Visualization of results

2. **Live Testing**
   - Paper trading mode on supported exchanges
   - Real-time execution without financial risk
   - Verification of strategy behavior in live market conditions

### Deployment Options

1. **Local Execution**
   - Running directly on user's machine
   - Requires continuous uptime for effective trading
   - Lightweight resource requirements

2. **VPS/Cloud Deployment**
   - Running on virtual private servers
   - Ensures 24/7 uptime and reliability
   - Reduces dependence on local internet connection

3. **Docker Containers**
   - Isolated, reproducible environment
   - Simplified deployment across platforms
   - Easy updates and version management

## Technical Constraints

### Exchange Limitations

1. **API Rate Limits**
   - Each exchange imposes request frequency limits
   - Strategy must operate within these constraints
   - Rate limiting and backoff mechanisms required

2. **Order Placement Latency**
   - Delay between order submission and execution
   - Market conditions may change during latency window
   - Strategy must account for execution uncertainty

3. **Market Precision**
   - Each market has specific tick sizes and quantity precision
   - Orders must conform to exchange-specific formatting
   - Price and quantity rounding required

### Performance Considerations

1. **Backtesting Speed**
   - Testing large datasets requires optimization
   - Rust implementation accelerates performance-critical operations
   - Memory efficiency important for large historical datasets

2. **Optimization Computational Requirements**
   - Evolutionary optimization is CPU-intensive
   - Parallel processing leverages multi-core systems
   - Long-running processes require stability

3. **Real-time Processing**
   - Live trading requires responsive order management
   - Market data processing must be efficient
   - Order book updates need low-latency handling

### Security Requirements

1. **API Key Management**
   - Secure storage of exchange credentials
   - Limited API permissions following principle of least privilege
   - No exposure of keys in logs or error reports

2. **Risk Management**
   - Position size limits and leverage controls
   - Loss prevention mechanisms
   - Automated circuit breakers for unusual market conditions

## Dependencies

### External APIs

1. **Exchange APIs**
   - Binance Futures API
   - Bybit API
   - OKX API
   - Bitget API
   - Gate.io API
   - Hyperliquid API
   - DefX API

2. **Market Data Providers**
   - Exchange-provided historical data
   - Real-time websocket feeds
   - Order book depth data

### System Dependencies

1. **Rust Toolchain**
   - Compiler (rustc)
   - Package manager (cargo)
   - Build utilities

2. **Python Environment**
   - Interpreter (3.8+)
   - Package management (pip)
   - Virtual environment

3. **Operating System Compatibility**
   - Linux (primary target)
   - macOS
   - Windows (with some limitations)

### Network Requirements

1. **Connectivity**
   - Stable internet connection
   - Low-latency access to exchange APIs
   - Sufficient bandwidth for market data

2. **Reliability**
   - Handling of intermittent connectivity issues
   - Reconnection logic
   - State recovery after disconnections

## Version History

The project is currently at version 7.3.11, indicating an established codebase with multiple major revisions. Key milestones in the development history include:

1. **Initial Python-only implementation**
2. **Addition of Rust components for performance**
3. **Expansion to multiple exchange support**
4. **Introduction of grid and trailing order strategies**
5. **Development of the optimization framework**
6. **Implementation of the Forager feature**
7. **Addition of unstucking mechanism**

Each major version has introduced significant improvements in functionality, performance, or reliability.
