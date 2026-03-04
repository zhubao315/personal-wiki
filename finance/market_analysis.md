# 市场分析方法

## 基本面分析

### 1. 宏观经济分析
```python
# 宏观经济指标分析
class MacroEconomicAnalysis:
    def __init__(self, economic_data):
        self.data = economic_data
    
    def gdp_analysis(self):
        """GDP分析"""
        gdp_growth = self.data['gdp'].pct_change(4)  # 同比增速
        gdp_trend = self.calculate_trend(gdp_growth)
        
        return {
            'current_growth': gdp_growth.iloc[-1],
            'trend': gdp_trend,
            'acceleration': self.calculate_acceleration(gdp_growth),
            'implications': self.analyze_gdp_implications(gdp_growth.iloc[-1])
        }
    
    def inflation_analysis(self):
        """通胀分析"""
        cpi = self.data['cpi']
        ppi = self.data['ppi']
        
        inflation_metrics = {
            'cpi_yoy': cpi.pct_change(12).iloc[-1],  # CPI同比
            'ppi_yoy': ppi.pct_change(12).iloc[-1],  # PPI同比
            'core_cpi': self.calculate_core_cpi(cpi),
            'inflation_trend': self.calculate_trend(cpi)
        }
        
        return inflation_metrics
    
    def monetary_policy_analysis(self):
        """货币政策分析"""
        interest_rates = self.data['interest_rate']
        money_supply = self.data['money_supply']
        
        policy_indicators = {
            'current_rate': interest_rates.iloc[-1],
            'rate_trend': self.calculate_trend(interest_rates),
            'money_growth': money_supply.pct_change(12).iloc[-1],
            'liquidity_conditions': self.analyze_liquidity(interest_rates, money_supply)
        }
        
        return policy_indicators
```

### 2. 行业分析
```python
# 行业分析
class IndustryAnalysis:
    def __init__(self, industry_data):
        self.industry_data = industry_data
    
    def industry_lifecycle_analysis(self, industry_name):
        """行业生命周期分析"""
        industry = self.industry_data[industry_name]
        
        lifecycle_indicators = {
            'growth_rate': industry['revenue_growth'].iloc[-1],
            'profit_margin': industry['profit_margin'].iloc[-1],
            'market_concentration': self.calculate_hhi(industry['market_shares']),
            'capital_expenditure': industry['capex'].iloc[-1],
            'rd_intensity': industry['rd_expense'].iloc[-1] / industry['revenue'].iloc[-1]
        }
        
        # 判断行业阶段
        if lifecycle_indicators['growth_rate'] > 0.2:
            stage = "growth"
        elif lifecycle_indicators['growth_rate'] > 0.05:
            stage = "maturity"
        else:
            stage = "decline"
        
        lifecycle_indicators['stage'] = stage
        return lifecycle_indicators
    
    def industry_attractiveness_analysis(self, industry_name):
        """行业吸引力分析"""
        industry = self.industry_data[industry_name]
        
        # 波特五力分析
        five_forces = {
            'competitive_rivalry': self.analyze_competitive_rivalry(industry),
            'supplier_power': self.analyze_supplier_power(industry),
            'buyer_power': self.analyze_buyer_power(industry),
            'threat_of_substitutes': self.analyze_substitute_threat(industry),
            'barriers_to_entry': self.analyze_barriers_to_entry(industry)
        }
        
        # 综合评分
        attractiveness_score = self.calculate_attractiveness_score(five_forces)
        
        return {
            'five_forces': five_forces,
            'attractiveness_score': attractiveness_score,
            'investment_recommendation': self.get_investment_recommendation(attractiveness_score)
        }
```

## 技术分析

### 1. 趋势分析
```python
# 趋势分析
class TrendAnalysis:
    def __init__(self, price_data):
        self.prices = price_data
    
    def moving_average_analysis(self, windows=[5, 10, 20, 50, 200]):
        """移动平均线分析"""
        mas = {}
        for window in windows:
            mas[f'ma_{window}'] = self.prices.rolling(window=window).mean()
        
        # 分析均线关系
        analysis = {}
        for i, window in enumerate(windows[:-1]):
            for j, next_window in enumerate(windows[i+1:], i+1):
                golden_cross = self.detect_golden_cross(mas[f'ma_{window}'], mas[f'ma_{next_window}'])
                death_cross = self.detect_death_cross(mas[f'ma_{window}'], mas[f'ma_{next_window}'])
                
                analysis[f'ma_{window}_vs_ma_{next_window}'] = {
                    'golden_cross': golden_cross,
                    'death_cross': death_cross,
                    'current_signal': 'bullish' if golden_cross else 'bearish' if death_cross else 'neutral'
                }
        
        return analysis
    
    def adx_analysis(self, period=14):
        """ADX趋势强度分析"""
        adx = self.calculate_adx(period)
        trend_strength = {
            'current_adx': adx.iloc[-1],
            'trend_strength': self.classify_trend_strength(adx.iloc[-1]),
            'trend_direction': self.get_trend_direction(),
            'trading_signal': self.get_adx_signal(adx.iloc[-1])
        }
        return trend_strength
```

### 2. 动量分析
```python
# 动量分析
class MomentumAnalysis:
    def __init__(self, price_data, volume_data):
        self.prices = price_data
        self.volumes = volume_data
    
    def rsi_analysis(self, period=14):
        """RSI相对强弱指数分析"""
        rsi = self.calculate_rsi(period)
        
        analysis = {
            'current_rsi': rsi.iloc[-1],
            'overbought_oversold': self.classify_rsi(rsi.iloc[-1]),
            'divergence': self.detect_rsi_divergence(rsi),
            'trend_strength': self.analyze_rsi_trend(rsi)
        }
        return analysis
    
    def macd_analysis(self, fast=12, slow=26, signal=9):
        """MACD分析"""
        macd, signal_line, histogram = self.calculate_macd(fast, slow, signal)
        
        analysis = {
            'macd_line': macd.iloc[-1],
            'signal_line': signal_line.iloc[-1],
            'histogram': histogram.iloc[-1],
            'signal': self.get_macd_signal(macd, signal_line),
            'momentum': self.analyze_macd_momentum(histogram)
        }
        return analysis
    
    def stochastic_oscillator_analysis(self, k_period=14, d_period=3):
        """随机指标分析"""
        k_line, d_line = self.calculate_stochastic(k_period, d_period)
        
        analysis = {
            'k_line': k_line.iloc[-1],
            'd_line': d_line.iloc[-1],
            'overbought_oversold': self.classify_stochastic(k_line.iloc[-1], d_line.iloc[-1]),
            'crossover': self.detect_stochastic_crossover(k_line, d_line),
            'divergence': self.detect_stochastic_divergence(k_line)
        }
        return analysis
```

### 3. 波动性分析
```python
# 波动性分析
class VolatilityAnalysis:
    def __init__(self, price_data):
        self.prices = price_data
    
    def bollinger_bands_analysis(self, period=20, std_dev=2):
        """布林带分析"""
        middle_band, upper_band, lower_band = self.calculate_bollinger_bands(period, std_dev)
        
        current_price = self.prices.iloc[-1]
        analysis = {
            'current_price': current_price,
            'middle_band': middle_band.iloc[-1],
            'upper_band': upper_band.iloc[-1],
            'lower_band': lower_band.iloc[-1],
            'position_in_bands': self.get_position_in_bands(current_price, upper_band.iloc[-1], middle_band.iloc[-1], lower_band.iloc[-1]),
            'band_width': upper_band.iloc[-1] - lower_band.iloc[-1],
            'volatility_signal': self.get_bollinger_signal(current_price, middle_band.iloc[-1], upper_band.iloc[-1], lower_band.iloc[-1])
        }
        return analysis
    
    def atr_analysis(self, period=14):
        """ATR波动性分析"""
        atr = self.calculate_atr(period)
        current_atr = atr.iloc[-1]
        atr_mean = atr.mean()
        
        analysis = {
            'current_atr': current_atr,
            'atr_ratio': current_atr / atr_mean,
            'volatility_level': self.classify_volatility(current_atr, atr_mean),
            'trading_signal': self.get_atr_signal(current_atr, atr_mean)
        }
        return analysis
```

## 量化分析

### 1. 因子分析
```python
# 因子分析
class FactorAnalysis:
    def __init__(self, stock_data, factor_data):
        self.stock_data = stock_data
        self.factor_data = factor_data
    
    def factor_correlation_analysis(self):
        """因子相关性分析"""
        correlations = {}
        for factor in self.factor_data.columns:
            correlation = self.stock_data['returns'].corr(self.factor_data[factor])
            correlations[factor] = correlation
        
        return correlations
    
    def factor_regression_analysis(self):
        """因子回归分析"""
        import statsmodels.api as sm
        
        X = self.factor_data
        y = self.stock_data['returns']
        X = sm.add_constant(X)
        
        model = sm.OLS(y, X).fit()
        
        analysis = {
            'model_summary': model.summary(),
            'factor_coefficients': model.params,
            'factor_significance': model.pvalues,
            'model_r_squared': model.rsquared,
            'model_adjusted_r_squared': model.rsquared_adj
        }
        return analysis
    
    def factor_performance_analysis(self, factor_weights):
        """因子性能分析"""
        factor_portfolio = self.calculate_factor_portfolio(factor_weights)
        
        performance = {
            'returns': factor_portfolio['returns'],
            'volatility': factor_portfolio['returns'].std(),
            'sharpe_ratio': self.calculate_sharpe_ratio(factor_portfolio['returns']),
            'max_drawdown': self.calculate_max_drawdown(factor_portfolio['returns']),
            'information_ratio': self.calculate_information_ratio(factor_portfolio['returns'], self.stock_data['returns'])
        }
        return performance
```

### 2. 机器学习分析
```python
# 机器学习市场分析
class MachineLearningAnalysis:
    def __init__(self, market_data, features):
        self.market_data = market_data
        self.features = features
    
    def prepare_features(self):
        """准备特征"""
        X = self.features.copy()
        
        # 添加技术指标
        X['ma_ratio'] = self.market_data['close'] / self.market_data['close'].rolling(20).mean()
        X['rsi'] = self.calculate_rsi(self.market_data['close'])
        X['volatility'] = self.market_data['close'].pct_change().rolling(20).std()
        X['volume_change'] = self.market_data['volume'].pct_change()
        
        # 处理缺失值
        X = X.dropna()
        
        return X
    
    def train_predictive_model(self, target_variable='returns'):
        """训练预测模型"""
        from sklearn.ensemble import RandomForestRegressor
        from sklearn.model_selection import train_test_split
        from sklearn.metrics import mean_squared_error, r2_score
        
        X = self.prepare_features()
        y = self.market_data[target_variable]
        
        # 对齐数据
        common_index = X.index.intersection(y.index)
        X = X.loc[common_index]
        y = y.loc[common_index]
        
        # 划分训练测试集
        X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, shuffle=False)
        
        # 训练模型
        model = RandomForestRegressor(n_estimators=100, random_state=42)
        model.fit(X_train, y_train)
        
        # 预测和评估
        predictions = model.predict(X_test)
        mse = mean_squared_error(y_test, predictions)
        r2 = r2_score(y_test, predictions)
        
        analysis = {
            'model': model,
            'predictions': predictions,
            'mse': mse,
            'r2_score': r2,
            'feature_importance': dict(zip(X.columns, model.feature_importances_)),
            'model_performance': self.assess_model_performance(r2, mse)
        }
        return analysis
```

## 情绪分析

### 1. 市场情绪指标
```python
# 市场情绪分析
class SentimentAnalysis:
    def __init__(self, sentiment_data):
        self.sentiment_data = sentiment_data
    
    def vix_analysis(self):
        """VIX恐慌指数分析"""
        vix = self.sentiment_data['vix']
        
        analysis = {
            'current_vix': vix.iloc[-1],
            'vix_level': self.classify_vix_level(vix.iloc[-1]),
            'vix_trend': self.calculate_trend(vix),
            'market_sentiment': self.analyze_vix_sentiment(vix.iloc[-1]),
            'trading_signal': self.get_vix_signal(vix.iloc[-1])
        }
        return analysis
    
    def put_call_ratio_analysis(self):
        """Put/Call比率分析"""
        put_call_ratio = self.sentiment_data['put_call_ratio']
        
        analysis = {
            'current_ratio': put_call_ratio.iloc[-1],
            'ratio_level': self.classify_put_call_ratio(put_call_ratio.iloc[-1]),
            'trend': self.calculate_trend(put_call_ratio),
            'sentiment_signal': self.get_put_call_signal(put_call_ratio.iloc[-1])
        }
        return analysis
    
    def investor_sentiment_analysis(self):
        """投资者情绪分析"""
        sentiment_surveys = self.sentiment_data['investor_sentiment']
        
        analysis = {
            'bullish_percentage': sentiment_surveys['bullish'].iloc[-1],
            'bearish_percentage': sentiment_surveys['bearish'].iloc[-1],
            'neutral_percentage': sentiment_surveys['neutral'].iloc[-1],
            'sentiment_balance': sentiment_surveys['bullish'].iloc[-1] - sentiment_surveys['bearish'].iloc[-1],
            'market_sentiment': self.classify_market_sentiment(sentiment_surveys.iloc[-1])
        }
        return analysis
```

### 2. 新闻情绪分析
```python
# 新闻情绪分析
class NewsSentimentAnalysis:
    def __init__(self, news_data):
        self.news_data = news_data
    
    def analyze_news_sentiment(self, symbol, days=7):
        """分析特定股票的新闻情绪"""
        recent_news = self.news_data[self.news_data['symbol'] == symbol].last(f'{days}D')
        
        if recent_news.empty:
            return {'error': 'No news data available'}
        
        # 计算情绪得分
        sentiment_scores = recent_news['sentiment_score']
        
        analysis = {
            'average_sentiment': sentiment_scores.mean(),
            'sentiment_std': sentiment_scores.std(),
            'sentiment_trend': self.calculate_sentiment_trend(sentiment_scores),
            'news_volume': len(recent_news),
            'impact_score': self.calculate_news_impact(sentiment_scores, recent_news['importance']),
            'trading_signal': self.get_news_sentiment_signal(sentiment_scores.mean())
        }
        return analysis
    
    def detect_sentiment_shifts(self, symbol, window=30):
        """检测情绪变化"""
        news_data = self.news_data[self.news_data['symbol'] == symbol]
        
        # 计算滚动情绪
        rolling_sentiment = news_data['sentiment_score'].rolling(window=window).mean()
        
        # 检测情绪突变
        sentiment_changes = rolling_sentiment.diff()
        significant_changes = sentiment_changes[abs(sentiment_changes) > sentiment_changes.std() * 2]
        
        analysis = {
            'rolling_sentiment': rolling_sentiment,
            'significant_changes': significant_changes,
            'current_sentiment': rolling_sentiment.iloc[-1],
            'sentiment_momentum': rolling_sentiment.diff().iloc[-1],
            'shift_detected': len(significant_changes) > 0
        }
        return analysis
```

## 组合分析

### 1. 资产配置分析
```python
# 组合分析
class PortfolioAnalysis:
    def __init__(self, portfolio_data, market_data):
        self.portfolio = portfolio_data
        self.market_data = market_data
    
    def asset_allocation_analysis(self):
        """资产配置分析"""
        current_allocation = self.portfolio.get_current_allocation()
        target_allocation = self.portfolio.get_target_allocation()
        
        analysis = {
            'current_allocation': current_allocation,
            'target_allocation': target_allocation,
            'deviation': self.calculate_allocation_deviation(current_allocation, target_allocation),
            'rebalance_needed': self.identify_rebalance_needs(current_allocation, target_allocation),
            'drift_analysis': self.analyze_allocation_drift(current_allocation, target_allocation)
        }
        return analysis
    
    def risk_contribution_analysis(self):
        """风险贡献分析"""
        portfolio_returns = self.portfolio.get_returns()
        asset_returns = self.portfolio.get_asset_returns()
        
        # 计算边际风险贡献
        marginal_risk_contribution = self.calculate_marginal_risk_contribution(
            portfolio_returns, asset_returns
        )
        
        analysis = {
            'total_portfolio_risk': portfolio_returns.std(),
            'marginal_risk_contribution': marginal_risk_contribution,
            'percentage_risk_contribution': self.calculate_percentage_risk_contribution(marginal_risk_contribution),
            'concentration_risk': self.analyze_concentration_risk(marginal_risk_contribution)
        }
        return analysis
```

### 2. 绩效归因分析
```python
# 绩效归因分析
class PerformanceAttribution:
    def __init__(self, portfolio_returns, benchmark_returns, factor_data):
        self.portfolio_returns = portfolio_returns
        self.benchmark_returns = benchmark_returns
        self.factor_data = factor_data
    
    def brinson_attribution(self):
        """Brinson绩效归因"""
        # 资产配置效应
        allocation_effect = self.calculate_allocation_effect()
        
        # 个股选择效应
        selection_effect = self.calculate_selection_effect()
        
        # 交互效应
        interaction_effect = self.calculate_interaction_effect()
        
        total_return = allocation_effect + selection_effect + interaction_effect
        
        attribution = {
            'allocation_effect': allocation_effect,
            'selection_effect': selection_effect,
            'interaction_effect': interaction_effect,
            'total_attribution': total_return,
            'excess_return': self.portfolio_returns - self.benchmark_returns,
            'attribution_summary': self.summarize_attribution(allocation_effect, selection_effect, interaction_effect)
        }
        return attribution
    
    def factor_attribution(self):
        """因子归因分析"""
        factor_exposures = self.calculate_factor_exposures()
        factor_returns = self.calculate_factor_returns()
        
        factor_contributions = factor_exposures * factor_returns
        
        attribution = {
            'factor_exposures': factor_exposures,
            'factor_returns': factor_returns,
            'factor_contributions': factor_contributions,
            'specific_return': self.calculate_specific_return(),
            'factor_performance': self.analyze_factor_performance(factor_contributions)
        }
        return attribution
```

## 实战案例

### 案例1: 2026年3月市场冲击分析
- **事件**: 霍尔木兹海峡危机
- **市场反应**: 
  - 股市单日下跌5%
  - VIX飙升至35
  - 油价上涨15%
- **分析要点**: 
  - 地缘政治风险溢价
  - 流动性冲击
  - 行业分化明显

### 案例2: 恒生科技指数分析
- **投资时机**: 2026年1月清仓
- **成功因素**: 
  - 技术分析显示超买
  - 基本面估值偏高
  - 政策风险上升
- **收益**: 50万浮盈实现

### 案例3: 组合再平衡案例
- **初始配置**: 股票60%，债券40%
- **市场变化**: 股票上涨至70%
- **再平衡**: 卖出部分股票，买入债券
- **效果**: 风险控制，收益优化

## 分析工具推荐

### 1. 数据分析工具
- **Python库**: pandas, numpy, matplotlib, seaborn
- **量化库**: zipline, backtrader, pyfolio
- **统计库**: scipy, statsmodels
- **机器学习**: scikit-learn, tensorflow, pytorch

### 2. 可视化工具
- **图表工具**: TradingView, Thinkorswim
- **数据可视化**: Tableau, Power BI, Plotly
- **专业分析**: Bloomberg, Reuters Eikon

### 3. 研究平台
- **量化平台**: QuantConnect, Quantopian
- **数据平台**: Quandl, Alpha Vantage, Yahoo Finance
- **研究社区**: Seeking Alpha, MarketWatch

---

*Last Updated: 2026-03-03*
*Version: 1.0*