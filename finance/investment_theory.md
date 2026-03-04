# 投资理论与策略

## 投资基础理论

### 1. 现代投资理论
- **现代投资组合理论(MPT)**: 通过分散投资降低风险
- **资本资产定价模型(CAPM)**: 资产预期收益与风险关系
- **有效市场假说(EMH)**: 市场价格反映所有可用信息
- **行为金融学**: 心理因素对投资决策的影响

### 2. 风险管理理论
- **风险平价理论**: 均衡配置各类资产风险
- **下行风险理论**: 重点关注损失风险
- **VaR模型**: 风险价值量化方法
- **压力测试**: 极端情况下的风险承受能力

## 投资策略体系

### 1. 资产配置策略
```python
# 资产配置示例
class AssetAllocation:
    def __init__(self, risk_profile):
        self.risk_profile = risk_profile
    
    def get_allocation(self):
        if self.risk_profile == 'conservative':
            return {
                'bonds': 0.6,
                'stocks': 0.3,
                'cash': 0.1
            }
        elif self.risk_profile == 'moderate':
            return {
                'bonds': 0.4,
                'stocks': 0.5,
                'cash': 0.1
            }
        else:  # aggressive
            return {
                'bonds': 0.2,
                'stocks': 0.7,
                'cash': 0.1
            }
```

### 2. 股票投资策略
- **价值投资**: 寻找被低估的股票
- **成长投资**: 投资高增长潜力公司
- **指数投资**: 跟踪市场指数
- **量化投资**: 基于数据和算法的投资

### 3. 择时策略
- **趋势跟踪**: 顺势而为
- **均值回归**: 价格回归均值
- **动量策略**: 强者恒强
- **反转策略**: 超跌反弹

## 技术分析

### 1. 趋势分析
```python
# 移动平均线策略
def moving_average_strategy(prices, short_window=5, long_window=20):
    short_ma = prices.rolling(window=short_window).mean()
    long_ma = prices.rolling(window=long_window).mean()
    
    signals = []
    for i in range(len(prices)):
        if i < long_window:
            signals.append(0)
        elif short_ma[i] > long_ma[i] and short_ma[i-1] <= long_ma[i-1]:
            signals.append(1)  # 买入信号
        elif short_ma[i] < long_ma[i] and short_ma[i-1] >= long_ma[i-1]:
            signals.append(-1)  # 卖出信号
        else:
            signals.append(0)
    
    return signals
```

### 2. 技术指标
- **MACD**: 趋势和动量指标
- **RSI**: 相对强弱指数
- **KDJ**: 随机指标
- **布林带**: 波动性指标

### 3. 图表形态
- **头肩顶/底**: 反转形态
- **双顶/底**: 反转形态
- **三角形**: 持续形态
- **旗形**: 持续形态

## 基本面分析

### 1. 财务分析
```python
# 财务指标分析
class FinancialAnalysis:
    def __init__(self, financial_data):
        self.data = financial_data
    
    def analyze_profitability(self):
        """盈利能力分析"""
        metrics = {
            'roe': self.data['net_income'] / self.data['equity'],  # 净资产收益率
            'roa': self.data['net_income'] / self.data['total_assets'],  # 总资产收益率
            'gross_margin': self.data['gross_profit'] / self.data['revenue'],  # 毛利率
            'net_margin': self.data['net_income'] / self.data['revenue']  # 净利率
        }
        return metrics
    
    def analyze_liquidity(self):
        """流动性分析"""
        metrics = {
            'current_ratio': self.data['current_assets'] / self.data['current_liabilities'],
            'quick_ratio': (self.data['current_assets'] - self.data['inventory']) / self.data['current_liabilities']
        }
        return metrics
```

### 2. 估值方法
- **PE比率**: 市盈率
- **PB比率**: 市净率
- **PS比率**: 市销率
- **DCF模型**: 现金流折现模型
- **DDM模型**: 股利折现模型

### 3. 行业分析
- **行业生命周期**: 初创、成长、成熟、衰退
- **波特五力**: 行业竞争分析
- **行业集中度**: 市场份额分布
- **行业增长率**: 增长速度分析

## 投资组合管理

### 1. 组合构建
```python
# 投资组合优化
import numpy as np
from scipy.optimize import minimize

class PortfolioOptimizer:
    def __init__(self, returns, cov_matrix):
        self.returns = returns
        self.cov_matrix = cov_matrix
    
    def optimize_portfolio(self, target_return):
        """优化投资组合"""
        n_assets = len(self.returns)
        
        def objective(weights):
            return np.sqrt(np.dot(weights.T, np.dot(self.cov_matrix, weights)))
        
        constraints = [
            {'type': 'eq', 'fun': lambda x: np.sum(x) - 1},  # 权重和为1
            {'type': 'eq', 'fun': lambda x: np.dot(x, self.returns) - target_return}  # 目标收益
        ]
        
        bounds = tuple((0, 1) for _ in range(n_assets))
        initial_weights = np.array([1/n_assets] * n_assets)
        
        result = minimize(objective, initial_weights, method='SLSQP', 
                         bounds=bounds, constraints=constraints)
        
        return result.x
```

### 2. 风险管理
- **分散化**: 降低单一资产风险
- **止损策略**: 控制损失范围
- **对冲策略**: 使用衍生品对冲风险
- **再平衡**: 定期调整组合配置

### 3. 绩效评估
- **夏普比率**: 风险调整后收益
- **特雷诺比率**: 单位风险收益
- **詹森指数**: 超额收益能力
- **信息比率**: 主动管理能力

## 行为金融学

### 1. 认知偏差
- **确认偏误**: 寻找支持自己观点的信息
- **损失厌恶**: 对损失比收益更敏感
- **锚定效应**: 过度依赖初始信息
- **过度自信**: 高估自己的判断能力

### 2. 情绪管理
- **贪婪与恐惧**: 市场情绪的极端表现
- **从众心理**: 跟随大众决策
- **处置效应**: 过早卖出盈利股票，持有亏损股票
- **心理账户**: 不同资金的心理分类

### 3. 决策改进
- **制定规则**: 减少情绪影响
- **分散投资**: 降低单一决策影响
- **定期复盘**: 总结经验教训
- **寻求专业建议**: 避免个人偏见

## 实战经验总结

### 1. 成功要素
- **长期视角**: 不被短期波动影响
- **纪律性**: 严格执行投资策略
- **学习能力**: 持续学习和改进
- **风险控制**: 永远把风险放在第一位

### 2. 常见错误
- **追涨杀跌**: 情绪化交易
- **过度交易**: 频繁买卖增加成本
- **集中投资**: 风险过于集中
- **忽略费用**: 交易成本侵蚀收益

### 3. 个人教训
- **2026年3月教训**: 地缘政治风险难以预测，需要建立应急机制
- **2026年1月经验**: 及时止盈，不要贪心
- **恒生科技案例**: 行业指数投资需要择时

## 投资计划模板

### 1. 投资目标设定
```yaml
investment_goals:
  short_term: "1-3年，流动性需求"
  medium_term: "3-5年，稳健增长"
  long_term: "5年以上，财富积累"
  
risk_tolerance:
  level: "moderate"  # conservative, moderate, aggressive
  max_drawdown: 0.2  # 最大回撤20%
  
time_horizon: "10年"
```

### 2. 资产配置计划
```yaml
asset_allocation:
  stocks:
    domestic: 0.3
    international: 0.2
  bonds:
    government: 0.2
    corporate: 0.1
  alternatives:
    real_estate: 0.1
    commodities: 0.05
  cash: 0.05
```

### 3. 执行策略
```yaml
execution_strategy:
  rebalance_frequency: "quarterly"  # 季度再平衡
  rebalance_threshold: 0.05  # 5%偏离阈值
  tax_consideration: true  # 考虑税收影响
  cost_optimization: true  # 成本优化
```

---

*Last Updated: 2026-03-03*
*Version: 1.0*