# Product Context for Passivbot

## Why Passivbot Exists

Passivbot was created to address several key challenges in cryptocurrency trading:

1. **Market Volatility Management**: Cryptocurrency markets are known for their high volatility. Passivbot provides a systematic approach to capitalize on these price movements rather than being victimized by them.

2. **Emotional Trading Elimination**: By automating trading decisions, Passivbot removes emotional biases that often lead to poor trading outcomes for manual traders.

3. **Continuous Market Participation**: Markets operate 24/7, and Passivbot enables traders to participate consistently without requiring constant monitoring.

4. **Complexity Reduction**: The bot simplifies the implementation of sophisticated trading strategies that would be difficult to execute manually.

5. **Risk Management Automation**: Passivbot incorporates risk management features that help protect capital while seeking profits.

## Problems Passivbot Solves

### For Cryptocurrency Traders

1. **Systematic Trading Implementation**: Converting trading ideas into systematic, executable strategies is challenging. Passivbot provides a framework for implementing grid-based strategies without requiring extensive programming knowledge.

2. **Parameter Optimization**: Finding optimal trading parameters through manual testing is time-consuming. Passivbot's optimization system automates this process using evolutionary algorithms.

3. **Exchange Integration**: Connecting to multiple exchanges with different APIs is complex. Passivbot handles these integrations, allowing traders to deploy the same strategy across different platforms.

4. **Stuck Position Management**: Traders often struggle with positions that move significantly against them. Passivbot's unstucking mechanism provides a systematic approach to handling these situations.

5. **Market Selection**: Identifying which markets offer the best trading opportunities is difficult. The Forager feature helps identify volatile markets that may provide higher profit potential.

### For the Market

1. **Liquidity Provision**: As a market maker, Passivbot contributes to market liquidity by placing limit orders that can be matched with market orders from other traders.

2. **Price Stabilization**: By acting as a contrarian (buying during price drops and selling during rises), Passivbot potentially helps reduce extreme price volatility.

## How Passivbot Should Work

### Core Operational Flow

1. **Market Connection**: Connect to supported cryptocurrency exchanges via API.

2. **Price Monitoring**: Continuously monitor market prices for configured trading pairs.

3. **Order Placement**: Place limit buy and sell orders based on grid and trailing order strategies.
   - Initial entries at specified price points
   - Re-entries (doubling down) when price moves against position
   - Take-profit orders at specified markup from average entry

4. **Position Management**: Track open positions and adjust orders as necessary.
   - Update take-profit orders after each re-entry
   - Apply unstucking mechanism to manage underperforming positions

5. **Risk Management**: Implement safeguards to protect account balance.
   - Limit losses to a percentage of peak balance
   - Prioritize unstucking positions with the smallest price gap

### User Experience Goals

1. **Minimal Intervention**: Once configured, Passivbot should operate autonomously with minimal need for user intervention.

2. **Transparency**: Provide clear visibility into trading decisions and performance through logs and visualization tools.

3. **Configurability**: Allow users to tailor the strategy to their risk tolerance and market views.

4. **Data-Driven Optimization**: Enable users to improve strategies based on historical backtesting and optimization.

5. **Exchange Flexibility**: Provide consistent functionality across multiple exchanges, allowing users to choose platforms based on their preferences and requirements.

6. **Deployment Options**: Support various deployment scenarios, from local execution to containerized deployments for improved reliability.

## Target Users

1. **Cryptocurrency Traders**: Individuals looking to automate their trading strategy with a systematic approach.

2. **Passive Investors**: Users seeking to generate potential returns on their cryptocurrency holdings without active management.

3. **Technical Users**: Developers and technically-inclined traders who can leverage the bot's configuration options and optimization capabilities.
