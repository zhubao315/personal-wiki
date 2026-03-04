# 风险管理指南

## 风险识别

### 1. 市场风险
- **价格风险**: 股票、债券等资产价格波动
- **利率风险**: 利率变动对投资价值的影响
- **汇率风险**: 汇率波动对跨国投资的影响
- **商品价格风险**: 原材料价格波动

### 2. 信用风险
- **违约风险**: 债券发行方无法按时偿付
- **交易对手风险**: 交易对方无法履行义务
- **评级下调风险**: 信用评级下降导致价值下跌

### 3. 流动性风险
- **市场流动性**: 无法及时买卖资产
- **资金流动性**: 现金流不足
- **融资风险**: 无法获得融资支持

### 4. 操作风险
- **系统故障**: 技术系统出现问题
- **人为错误**: 操作失误导致损失
- **欺诈风险**: 内部或外部欺诈行为
- **合规风险**: 违反法律法规

### 5. 地缘政治风险
- **政治风险**: 政权更迭、政策变化
- **战争风险**: 冲突导致市场动荡
- **贸易风险**: 贸易摩擦和制裁
- **能源风险**: 能源供应中断

## 风险评估方法

### 1. 量化评估
```python
# 风险评估模型
import numpy as np
import pandas as pd

class RiskAssessment:
    def __init__(self, returns_data):
        self.returns = returns_data
    
    def calculate_var(self, confidence_level=0.95):
        """计算VaR（风险价值）"""
        sorted_returns = np.sort(self.returns)
        index = int((1 - confidence_level) * len(sorted_returns))
        var = -sorted_returns[index]
        return var
    
    def calculate_cvar(self, confidence_level=0.95):
        """计算CVaR（条件风险价值）"""
        var = self.calculate_var(confidence_level)
        cvar = -self.returns[self.returns <= -var].mean()
        return cvar
    
    def calculate_beta(self, market_returns):
        """计算Beta系数"""
        covariance = np.cov(self.returns, market_returns)[0, 1]
        market_variance = np.var(market_returns)
        beta = covariance / market_variance
        return beta
    
    def calculate_sharpe_ratio(self, risk_free_rate=0.02):
        """计算夏普比率"""
        excess_returns = self.returns - risk_free_rate / 252
        sharpe_ratio = np.mean(excess_returns) / np.std(self.returns)
        return sharpe_ratio
```

### 2. 压力测试
```python
# 压力测试模型
class StressTest:
    def __init__(self, portfolio, market_data):
        self.portfolio = portfolio
        self.market_data = market_data
    
    def scenario_analysis(self, scenarios):
        """情景分析"""
        results = {}
        
        for scenario_name, scenario_params in scenarios.items():
            # 利率冲击
            if 'interest_rate_shock' in scenario_params:
                impact = self.interest_rate_impact(
                    scenario_params['interest_rate_shock']
                )
                results[f"{scenario_name}_interest"] = impact
            
            # 股市暴跌
            if 'equity_crash' in scenario_params:
                impact = self.equity_crash_impact(
                    scenario_params['equity_crash']
                )
                results[f"{scenario_name}_equity"] = impact
            
            # 汇率波动
            if 'exchange_rate_shock' in scenario_params:
                impact = self.exchange_rate_impact(
                    scenario_params['exchange_rate_shock']
                )
                results[f"{scenario_name}_fx"] = impact
        
        return results
    
    def historical_scenarios(self):
        """历史情景测试"""
        # 2008年金融危机
        crisis_scenario = {
            'equity_crash': -0.5,  # 股市下跌50%
            'interest_rate_shock': -0.03,  # 利率下降3%
            'credit_spread_widening': 0.05  # 信用利差扩大5%
        }
        
        return self.scenario_analysis({'financial_crisis': crisis_scenario})
```

### 3. 敏感性分析
```python
# 敏感性分析
class SensitivityAnalysis:
    def __init__(self, portfolio):
        self.portfolio = portfolio
    
    def sensitivity_to_market(self, market_change_range=(-0.1, 0.1), steps=10):
        """市场敏感性分析"""
        changes = np.linspace(market_change_range[0], market_change_range[1], steps)
        portfolio_values = []
        
        for change in changes:
            portfolio_value = self.calculate_portfolio_impact(change)
            portfolio_values.append(portfolio_value)
        
        return {
            'market_changes': changes,
            'portfolio_values': portfolio_values,
            'sensitivity': np.mean(np.diff(portfolio_values) / np.diff(changes))
        }
    
    def sensitivity_to_interest_rate(self, rate_change_range=(-0.02, 0.02), steps=10):
        """利率敏感性分析"""
        changes = np.linspace(rate_change_range[0], rate_change_range[1], steps)
        portfolio_values = []
        
        for change in changes:
            portfolio_value = self.calculate_interest_rate_impact(change)
            portfolio_values.append(portfolio_value)
        
        return {
            'rate_changes': changes,
            'portfolio_values': portfolio_values,
            'duration': np.mean(np.diff(portfolio_values) / np.diff(changes))
        }
```

## 风险控制策略

### 1. 分散化策略
```python
# 分散化分析
class DiversificationStrategy:
    def __init__(self, assets_data):
        self.assets = assets_data
    
    def calculate_correlation_matrix(self):
        """计算相关系数矩阵"""
        returns = self.assets.pct_change().dropna()
        correlation_matrix = returns.corr()
        return correlation_matrix
    
    def calculate_portfolio_volatility(self, weights):
        """计算组合波动率"""
        returns = self.assets.pct_change().dropna()
        cov_matrix = returns.cov()
        portfolio_vol = np.sqrt(np.dot(weights.T, np.dot(cov_matrix, weights)))
        return portfolio_vol
    
    def optimize_diversification(self):
        """优化分散化配置"""
        n_assets = len(self.assets.columns)
        correlation_matrix = self.calculate_correlation_matrix()
        
        # 寻找低相关性的资产组合
        low_correlation_pairs = []
        for i in range(n_assets):
            for j in range(i+1, n_assets):
                corr = correlation_matrix.iloc[i, j]
                if corr < 0.3:  # 低相关性
                    low_correlation_pairs.append((i, j, corr))
        
        return low_correlation_pairs
```

### 2. 对冲策略
```python
# 对冲策略
class HedgingStrategy:
    def __init__(self, portfolio, derivatives_data):
        self.portfolio = portfolio
        self.derivatives = derivatives_data
    
    def delta_hedging(self, options_positions):
        """Delta对冲"""
        total_delta = 0
        for option in options_positions:
            delta = self.calculate_delta(option)
            total_delta += delta
        
        # 计算需要对冲的股票数量
        hedge_shares = -total_delta
        return hedge_shares
    
    def portfolio_insurance(self, floor_value, current_value):
        """投资组合保险"""
        cushion = current_value - floor_value
        if cushion <= 0:
            return 0  # 全部投资债券
        
        # 根据风险乘数确定股票配置
        risk_multiplier = 2
        stock_allocation = min(cushion * risk_multiplier / current_value, 0.8)
        
        return stock_allocation
```

### 3. 止损策略
```python
# 止损策略
class StopLossStrategy:
    def __init__(self, risk_tolerance=0.02):
        self.risk_tolerance = risk_tolerance
    
    def calculate_stop_loss(self, entry_price, volatility, method='fixed'):
        """计算止损价格"""
        if method == 'fixed':
            # 固定百分比止损
            stop_loss = entry_price * (1 - self.risk_tolerance)
        elif method == 'volatility':
            # 波动性止损
            atr = self.calculate_atr(volatility, period=14)
            stop_loss = entry_price - 2 * atr
        elif method == 'trailing':
            # 移动止损
            stop_loss = self.calculate_trailing_stop(entry_price, volatility)
        else:
            stop_loss = entry_price * (1 - 0.1)  # 默认10%止损
        
        return stop_loss
    
    def trailing_stop_update(self, current_price, highest_price, trail_percentage=0.1):
        """更新移动止损"""
        trailing_stop = highest_price * (1 - trail_percentage)
        if current_price > trailing_stop:
            return current_price * (1 - trail_percentage)
        else:
            return trailing_stop
```

## 风险监控体系

### 1. 实时监控
```python
# 风险监控系统
class RiskMonitor:
    def __init__(self, portfolio, risk_limits):
        self.portfolio = portfolio
        self.risk_limits = risk_limits
        self.alerts = []
    
    def real_time_monitoring(self, market_data):
        """实时监控"""
        current_risks = self.calculate_current_risks(market_data)
        
        # 检查风险限制
        for risk_type, limit in self.risk_limits.items():
            current_value = current_risks.get(risk_type, 0)
            if current_value > limit:
                self.trigger_alert(risk_type, current_value, limit)
        
        return current_risks
    
    def calculate_current_risks(self, market_data):
        """计算当前风险"""
        risks = {
            'market_risk': self.calculate_market_risk(market_data),
            'credit_risk': self.calculate_credit_risk(),
            'liquidity_risk': self.calculate_liquidity_risk(),
            'concentration_risk': self.calculate_concentration_risk()
        }
        return risks
```

### 2. 预警系统
```python
# 风险预警
class RiskAlertSystem:
    def __init__(self):
        self.alert_levels = {
            'low': 0.6,      # 60%预警
            'medium': 0.8,   # 80%预警
            'high': 0.9,     # 90%预警
            'critical': 1.0  # 100%警告
        }
    
    def check_risk_levels(self, current_risks, risk_limits):
        """检查风险级别"""
        alerts = []
        
        for risk_type, current_value in current_risks.items():
            limit = risk_limits.get(risk_type, 1.0)
            ratio = current_value / limit
            
            if ratio >= self.alert_levels['critical']:
                level = 'critical'
            elif ratio >= self.alert_levels['high']:
                level = 'high'
            elif ratio >= self.alert_levels['medium']:
                level = 'medium'
            elif ratio >= self.alert_levels['low']:
                level = 'low'
            else:
                level = 'normal'
            
            if level != 'normal':
                alerts.append({
                    'risk_type': risk_type,
                    'level': level,
                    'value': current_value,
                    'limit': limit,
                    'ratio': ratio
                })
        
        return alerts
```

### 3. 报告系统
```python
# 风险报告
class RiskReporting:
    def __init__(self, portfolio):
        self.portfolio = portfolio
    
    def generate_daily_report(self, market_data):
        """生成日报"""
        report = {
            'date': pd.Timestamp.now().date(),
            'portfolio_value': self.portfolio.value,
            'daily_pnl': self.calculate_daily_pnl(),
            'risk_metrics': self.calculate_risk_metrics(market_data),
            'compliance_status': self.check_compliance(),
            'alerts': self.check_alerts()
        }
        return report
    
    def generate_weekly_report(self):
        """生成周报"""
        report = {
            'week': pd.Timestamp.now().isocalendar()[1],
            'weekly_pnl': self.calculate_weekly_pnl(),
            'risk_trends': self.analyze_risk_trends(),
            'portfolio_changes': self.get_portfolio_changes(),
            'market_analysis': self.analyze_market_conditions()
        }
        return report
```

## 具体风险管理实践

### 1. 股票投资风险管理
- **分散化**: 持有15-20只不同行业的股票
- **止损**: 设置10-15%的止损线
- **仓位控制**: 单只股票不超过总仓位的5%
- **Beta管理**: 控制组合整体Beta在合理范围

### 2. 债券投资风险管理
- **信用评级**: 主要投资AAA级债券
- **久期管理**: 控制组合久期在3-5年
- **利率风险**: 使用利率衍生品对冲
- **流动性**: 保持一定比例的流动性好的债券

### 3. 衍生品风险管理
- **Delta中性**: 保持Delta中性或接近中性
- **希腊字母监控**: 实时监控Delta、Gamma、Vega
- **保证金管理**: 保持充足的保证金
- **到期管理**: 及时处理到期合约

### 4. 地缘政治风险管理
- **地域分散**: 投资多个国家和区域
- **避险资产**: 配置黄金、美元等避险资产
- **动态调整**: 根据风险事件及时调整配置
- **情景分析**: 定期进行地缘政治情景分析

## 风险管理制度

### 1. 风险政策
```yaml
risk_policies:
  risk_appetite: "moderate"  # 风险偏好
  max_portfolio_volatility: 0.15  # 最大组合波动率15%
  max_drawdown_limit: 0.20  # 最大回撤20%
  max_leverage: 2.0  # 最大杠杆2倍
  liquidity_requirement: 0.1  # 流动性要求10%
```

### 2. 职责分工
```yaml
risk_organization:
  risk_committee: "制定风险政策和限额"
  risk_manager: "日常风险监控和管理"
  traders: "执行交易并报告风险"
  compliance: "合规检查和监督"
  internal_audit: "风险管理体系审计"
```

### 3. 应急预案
```yaml
emergency_procedures:
  market_crash:
    trigger: "单日下跌超过10%"
    actions:
      - "启动压力测试"
      - "检查流动性状况"
      - "降低风险敞口"
      - "寻求避险资产"
  
  liquidity_crisis:
    trigger: "融资成本大幅上升"
    actions:
      - "变现非核心资产"
      - "寻求紧急融资"
      - "暂停新增投资"
      - "加强现金流管理"
```

## 实战案例

### 案例1: 2026年3月地缘政治风险
- **风险事件**: 霍尔木兹海峡危机
- **影响**: 股市暴跌，浮亏11.5万
- **教训**: 
  - 需要建立地缘政治风险应急机制
  - 保持一定比例的避险资产
  - 不要过度集中投资

### 案例2: 2026年1月投资决策
- **成功经验**: 恒生科技清仓，浮盈50万
- **成功因素**: 
  - 及时止盈，不贪心
  - 技术分析结合基本面
  - 严格执行投资策略

### 案例3: 分散化效果
- **组合配置**: 股票40%，债券40%，黄金10%，现金10%
- **风险降低**: 相比单一股票投资，波动率降低30%
- **收益稳定**: 年化收益率8-10%，回撤控制在15%以内

---

*Last Updated: 2026-03-03*
*Version: 1.0*