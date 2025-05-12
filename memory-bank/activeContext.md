# Active Context for Passivbot

## Current Work Focus

The current focus is on understanding and documenting the Passivbot project structure, architecture, and functionality, with a specific emphasis on the configuration parameters and their effects on trading strategy. This comprehensive documentation serves as a foundation for future development, maintenance, and enhancement work.

### Primary Areas of Focus

1. **Configuration System Understanding**
   - Analyzing template.json parameters in depth
   - Documenting the relationship between parameters and trading behavior
   - Establishing configuration guidelines for different market conditions
   - Understanding optimization strategies for parameter tuning

2. **Trading Strategy Implementation**
   - Documenting the grid-based and trailing order systems
   - Analyzing the Martingale-inspired doubling down approach
   - Understanding the unstucking mechanism for position management
   - Mapping the Forager feature's dynamic coin selection system

3. **System Architecture Understanding**
   - Analyzing the relationship between Python and Rust components
   - Understanding the exchange adapter system
   - Mapping the optimization and backtesting architecture

4. **Risk Management System**
   - Documenting wallet exposure controls
   - Understanding position sizing algorithms
   - Analyzing diversification strategies
   - Documenting circuit breaker mechanisms

## Configuration Parameters Guide

### Core Strategy Parameters Explained

Passivbot's behavior is primarily controlled through the parameters in the `bot` section of the configuration. Here's a guide to effectively configuring these parameters:

#### 1. Entry Strategy Parameters

These parameters control when and how the bot enters positions:

| Parameter | Description | Recommended Range | Effect |
|-----------|-------------|-------------------|--------|
| `entry_initial_qty_pct` | Size of initial entry as percentage of max position | 0.005-0.02 | Smaller values create more re-entry opportunities |
| `entry_initial_ema_dist` | Distance from EMA for initial entry | -0.1 to 0.001 (long) | More negative values place entries further from price |
| `entry_grid_spacing_pct` | Percentage distance between grid entries | 0.001-0.05 | Larger values space entries further apart |
| `entry_grid_spacing_weight` | Modifier that increases spacing as position grows | 0-10 | Higher values increase spacing for larger positions |
| `entry_grid_double_down_factor` | Size multiplier for each subsequent entry | 0.01-4 | Values >1 create increasing entry sizes |

#### 2. Trailing Entry Parameters

These parameters control when and how trailing entries are placed:

| Parameter | Description | Recommended Range | Effect |
|-----------|-------------|-------------------|--------|
| `entry_trailing_threshold_pct` | Required price move before trailing starts | -0.01-0.1 | Higher values require larger moves |
| `entry_trailing_retracement_pct` | Required retracement to trigger entry | 0.0001-0.1 | Higher values wait for deeper retracements |
| `entry_trailing_grid_ratio` | Balance between grid and trailing entries | -1 to 1 | 0=grid only, 1=trailing only |

#### 3. Close Strategy Parameters

These parameters control when and how positions are closed:

| Parameter | Description | Recommended Range | Effect |
|-----------|-------------|-------------------|--------|
| `close_grid_min_markup` | Minimum profit percentage for take-profit | 0.001-0.03 | Higher values target larger profits |
| `close_grid_markup_range` | Range for distributing take-profit orders | 0-0.03 | Higher values spread TP orders wider |
| `close_grid_qty_pct` | Percentage of position to close at each level | 0.05-1 | Lower values close position in smaller increments |

#### 4. Trailing Close Parameters

These parameters control trailing take-profit behavior:

| Parameter | Description | Recommended Range | Effect |
|-----------|-------------|-------------------|--------|
| `close_trailing_threshold_pct` | Required favorable move before trailing | -0.01-0.1 | Higher values require larger favorable moves |
| `close_trailing_retracement_pct` | Retracement to trigger close | 0.0001-0.1 | Higher values wait for deeper retracements |
| `close_trailing_grid_ratio` | Balance between grid and trailing closes | -1 to 1 | 0=grid only, 1=trailing only |
| `close_trailing_qty_pct` | Percentage of position to close with trailing | 0.05-1 | Lower values close position in smaller increments |

#### 5. Risk Management Parameters

These parameters control overall risk exposure:

| Parameter | Description | Recommended Range | Effect |
|-----------|-------------|-------------------|--------|
| `total_wallet_exposure_limit` | Maximum position value ratio to balance | 0.01-2 | Higher values increase capital utilization |
| `n_positions` | Maximum number of concurrent positions | 1-30 | Higher values increase diversification |
| `enforce_exposure_limit` | Whether to auto-reduce oversized positions | true/false | Enables automatic risk control |

#### 6. Unstucking Parameters

These parameters control how underwater positions are managed:

| Parameter | Description | Recommended Range | Effect |
|-----------|-------------|-------------------|--------|
| `unstuck_threshold` | Position ratio that triggers unstucking | 0.4-0.95 | Lower values trigger unstucking earlier |
| `unstuck_ema_dist` | EMA distance for unstucking orders | -0.1-0.01 | Controls price for unstucking orders |
| `unstuck_close_pct` | Portion of position to close when unstucking | 0.001-0.1 | Higher values close larger portions |
| `unstuck_loss_allowance_pct` | Maximum allowed losses relative to peak | 0.001-0.05 | Higher values allow more realized losses |

### Configuration Patterns for Different Market Conditions

#### Sideways/Ranging Markets

```json
{
  "entry_grid_spacing_pct": 0.01,
  "entry_grid_spacing_weight": 1.5,
  "close_grid_min_markup": 0.005,
  "close_grid_markup_range": 0.01,
  "entry_trailing_grid_ratio": 0.0,
  "close_trailing_grid_ratio": 0.0
}
```

Explanation: In ranging markets, pure grid strategy works well. Small markups allow capturing frequent small profits, while moderate grid spacing catches oscillations.

#### Volatile Markets

```json
{
  "entry_trailing_threshold_pct": 0.03,
  "entry_trailing_retracement_pct": 0.02,
  "close_trailing_threshold_pct": 0.03,
  "close_trailing_retracement_pct": 0.015,
  "entry_trailing_grid_ratio": 0.7,
  "close_trailing_grid_ratio": 0.7
}
```

Explanation: In volatile markets, trailing orders perform better by waiting for significant moves and catching retracements. Higher grid ratio prioritizes trailing orders over grid orders.

#### Trending Markets

```json
{
  "unstuck_threshold": 0.6,
  "unstuck_ema_dist": -0.05,
  "unstuck_close_pct": 0.05,
  "unstuck_loss_allowance_pct": 0.01,
  "entry_grid_double_down_factor": 1.5,
  "close_grid_min_markup": 0.01
}
```

Explanation: In trending markets, positions can get stuck more easily. These settings provide more aggressive unstucking, allowing the bot to realize smaller losses and redeploy capital more effectively.

## Optimization Guidelines

### Key Metrics for Optimization

When optimizing Passivbot parameters, these are the primary metrics to consider:

1. **Return Metrics**
   - Average Daily Gain (adg): Geometric mean of daily returns
   - Median Daily Gain (mdg): Median of daily returns
   - Both metrics should be positive with values above 0.001 (0.1% daily)

2. **Risk Metrics**
   - Maximum Drawdown: Should typically be kept below 30-40%
   - Sharpe/Sortino Ratio: Higher values (>1.0) indicate better risk-adjusted returns
   - Loss/Profit Ratio: Lower values indicate more efficient trading

3. **Position Metrics**
   - Maximum Position Duration: Avoid very long-held positions
   - Average Number of Positions: Should align with diversification goals

### Optimization Workflow

1. **Define Objectives**
   - Determine primary goals (e.g., max return, min drawdown, or balanced)
   - Select appropriate scoring metrics in the optimize section

2. **Configure Bounds**
   - Set reasonable parameter ranges in optimize.bounds
   - Narrow ranges for parameters you want to constrain
   - Widen ranges for parameters you want to explore

3. **Run Optimization**
   - Use a sufficient number of iterations (300,000+)
   - Ensure adequate historical data coverage
   - Consider using multiple markets for robustness

4. **Analyze Results**
   - Compare Pareto-optimal configurations
   - Test top configurations on out-of-sample data
   - Consider performance across different market conditions

5. **Refine Parameters**
   - Start with optimizer-suggested values
   - Make manual adjustments based on market understanding
   - Consider creating specialized configs for different market types

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
   - Configuration parameters have been documented with their effects

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
