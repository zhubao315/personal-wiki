# 心率数据分析

## 心率基础理论

### 1. 心率生理学
- **静息心率**: 安静状态下的心率，反映基础心血管健康
- **最大心率**: 运动时能达到的最高心率
- **心率恢复**: 运动后心率恢复速度
- **心率变异性**: 心率变化的复杂性和自主神经平衡

### 2. 心率计算公式
```python
# 心率计算工具
class HeartRateCalculator:
    def __init__(self, age, resting_hr):
        self.age = age
        self.resting_hr = resting_hr
    
    def calculate_max_hr(self, method='traditional'):
        """计算最大心率"""
        if method == 'traditional':
            # 传统公式: 220 - 年龄
            return 220 - self.age
        elif method == 'improved':
            # 改进公式: 208 - 0.7 * 年龄
            return 208 - 0.7 * self.age
        elif method == 'women':
            # 女性专用: 206 - 0.88 * 年龄
            return 206 - 0.88 * self.age
        else:
            return 220 - self.age
    
    def calculate_hr_reserve(self, max_hr):
        """计算心率储备"""
        return max_hr - self.resting_hr
    
    def calculate_target_hr(self, intensity, max_hr):
        """计算目标心率"""
        hr_reserve = self.calculate_hr_reserve(max_hr)
        return self.resting_hr + hr_reserve * intensity
    
    def calculate_hrr_zones(self, max_hr):
        """计算心率储备区间"""
        hr_reserve = self.calculate_hr_reserve(max_hr)
        
        zones = {
            'Zone 1 (50-60% HRR)': {
                'range': (self.resting_hr + hr_reserve * 0.5, self.resting_hr + hr_reserve * 0.6),
                'intensity': '非常轻松',
                'training_effect': '恢复和基本有氧能力'
            },
            'Zone 2 (60-70% HRR)': {
                'range': (self.resting_hr + hr_reserve * 0.6, self.resting_hr + hr_reserve * 0.7),
                'intensity': '轻松',
                'training_effect': '有氧基础建设'
            },
            'Zone 3 (70-80% HRR)': {
                'range': (self.resting_hr + hr_reserve * 0.7, self.resting_hr + hr_reserve * 0.8),
                'intensity': '中等',
                'training_effect': '乳酸阈值提升'
            },
            'Zone 4 (80-90% HRR)': {
                'range': (self.resting_hr + hr_reserve * 0.8, self.resting_hr + hr_reserve * 0.9),
                'intensity': '困难',
                'training_effect': '速度和耐力'
            },
            'Zone 5 (90-100% HRR)': {
                'range': (self.resting_hr + hr_reserve * 0.9, max_hr),
                'intensity': '非常困难',
                'training_effect': '最大摄氧量提升'
            }
        }
        return zones
```

## 心率数据分析方法

### 1. 静息心率分析
```python
# 静息心率分析
class RestingHeartRateAnalysis:
    def __init__(self, resting_hr_data):
        self.resting_hr_data = resting_hr_data
    
    def analyze_resting_hr_trends(self):
        """分析静息心率趋势"""
        trends = {
            'current_resting_hr': self.resting_hr_data.iloc[-1],
            'weekly_average': self.resting_hr_data.tail(7).mean(),
            'monthly_average': self.resting_hr_data.tail(30).mean(),
            'trend_direction': self.calculate_trend_direction(),
            'fitness_level': self.assess_fitness_level(),
            'health_indicators': self.analyze_health_indicators()
        }
        return trends
    
    def calculate_trend_direction(self):
        """计算趋势方向"""
        recent_hr = self.resting_hr_data.tail(7).mean()
        previous_hr = self.resting_hr_data.tail(14).head(7).mean()
        
        if recent_hr < previous_hr - 2:
            return 'decreasing'  # 下降趋势，健康状况改善
        elif recent_hr > previous_hr + 2:
            return 'increasing'  # 上升趋势，可能需要关注
        else:
            return 'stable'  # 稳定趋势
    
    def assess_fitness_level(self):
        """评估健康水平"""
        current_hr = self.resting_hr_data.iloc[-1]
        
        if current_hr < 60:
            return 'excellent'  # 优秀，运动员水平
        elif current_hr < 70:
            return 'good'  # 良好
        elif current_hr < 80:
            return 'average'  # 一般
        else:
            return 'below_average'  # 较差
    
    def analyze_health_indicators(self):
        """分析健康指标"""
        current_hr = self.resting_hr_data.iloc[-1]
        variability = self.resting_hr_data.tail(30).std()
        
        indicators = {
            'cardiovascular_fitness': self.assess_cardiovascular_fitness(current_hr),
            'recovery_status': self.assess_recovery_status(current_hr),
            'stress_level': self.assess_stress_level(current_hr, variability),
            'illness_indicator': self.assess_illness_indicator(current_hr)
        }
        return indicators
```

### 2. 运动心率分析
```python
# 运动心率分析
class ExerciseHeartRateAnalysis:
    def __init__(self, exercise_hr_data, max_hr, resting_hr):
        self.exercise_hr_data = exercise_hr_data
        self.max_hr = max_hr
        self.resting_hr = resting_hr
    
    def analyze_exercise_intensity(self):
        """分析运动强度"""
        hr_reserve = self.max_hr - self.resting_hr
        
        analysis = {
            'average_hr': self.exercise_hr_data.mean(),
            'max_hr': self.exercise_hr_data.max(),
            'min_hr': self.exercise_hr_data.min(),
            'hr_reserve': hr_reserve,
            'intensity_zones': self.calculate_time_in_zones(),
            'average_intensity': self.calculate_average_intensity(),
            'training_effectiveness': self.assess_training_effectiveness()
        }
        return analysis
    
    def calculate_time_in_zones(self):
        """计算在各心率区间的时间"""
        hr_reserve = self.max_hr - self.resting_hr
        
        zones = {
            'Zone 1 (50-60% HRR)': 0,
            'Zone 2 (60-70% HRR)': 0,
            'Zone 3 (70-80% HRR)': 0,
            'Zone 4 (80-90% HRR)': 0,
            'Zone 5 (90-100% HRR)': 0
        }
        
        for hr in self.exercise_hr_data:
            intensity = (hr - self.resting_hr) / hr_reserve
            
            if 0.5 <= intensity < 0.6:
                zones['Zone 1 (50-60% HRR)'] += 1
            elif 0.6 <= intensity < 0.7:
                zones['Zone 2 (60-70% HRR)'] += 1
            elif 0.7 <= intensity < 0.8:
                zones['Zone 3 (70-80% HRR)'] += 1
            elif 0.8 <= intensity < 0.9:
                zones['Zone 4 (80-90% HRR)'] += 1
            elif 0.9 <= intensity <= 1.0:
                zones['Zone 5 (90-100% HRR)'] += 1
        
        # 转换为百分比
        total_time = len(self.exercise_hr_data)
        for zone in zones:
            zones[zone] = zones[zone] / total_time * 100
        
        return zones
    
    def assess_training_effectiveness(self):
        """评估训练效果"""
        time_in_zones = self.calculate_time_in_zones()
        
        # 根据训练目标评估效果
        if time_in_zones['Zone 2 (60-70% HRR)'] > 60:
            return 'good_aerobic_base'  # 有氧基础训练效果好
        elif time_in_zones['Zone 3 (70-80% HRR)'] > 30:
            return 'good_threshold_training'  # 阈值训练效果好
        elif time_in_zones['Zone 4 (80-90% HRR)'] + time_in_zones['Zone 5 (90-100% HRR)'] > 20:
            return 'good_speed_training'  # 速度训练效果好
        else:
            return 'mixed_effectiveness'  # 混合效果
```

### 3. 心率恢复分析
```python
# 心率恢复分析
class HeartRateRecoveryAnalysis:
    def __init__(self, post_exercise_hr_data):
        self.post_exercise_hr_data = post_exercise_hr_data
    
    def calculate_hr_recovery(self, time_intervals=[1, 2, 3, 5, 10]):
        """计算心率恢复"""
        recovery_data = {}
        
        max_hr = self.post_exercise_hr_data.iloc[0]  # 运动结束时的最高心率
        
        for interval in time_intervals:
            if interval < len(self.post_exercise_hr_data):
                hr_at_interval = self.post_exercise_hr_data.iloc[interval]
                recovery_amount = max_hr - hr_at_interval
                recovery_rate = recovery_amount / interval
                
                recovery_data[f'{interval}_min'] = {
                    'hr_at_interval': hr_at_interval,
                    'recovery_amount': recovery_amount,
                    'recovery_rate': recovery_rate
                }
        
        return recovery_data
    
    def assess_recovery_quality(self):
        """评估恢复质量"""
        recovery_1min = self.post_exercise_hr_data.iloc[0] - self.post_exercise_hr_data.iloc[1]
        recovery_2min = self.post_exercise_hr_data.iloc[0] - self.post_exercise_hr_data.iloc[2]
        
        if recovery_1min > 20 and recovery_2min > 40:
            return 'excellent'  # 优秀恢复能力
        elif recovery_1min > 15 and recovery_2min > 30:
            return 'good'  # 良好恢复能力
        elif recovery_1min > 10 and recovery_2min > 20:
            return 'average'  # 一般恢复能力
        else:
            return 'poor'  # 恢复能力较差
```

## 心率变异性分析

### 1. HRV基础理论
```python
# 心率变异性分析
class HRVAnalysis:
    def __init__(self, rr_intervals):
        self.rr_intervals = rr_intervals  # R-R间期数据
    
    def calculate_time_domain_metrics(self):
        """计算时域指标"""
        import numpy as np
        
        metrics = {
            'SDNN': np.std(self.rr_intervals),  # R-R间期标准差
            'RMSSD': self.calculate_rmssd(),  # 相邻R-R间期差值的均方根
            'pNN50': self.calculate_pnn50(),  # 相邻R-R间期差值大于50ms的百分比
            'Mean_RR': np.mean(self.rr_intervals),
            'Median_RR': np.median(self.rr_intervals)
        }
        
        return metrics
    
    def calculate_frequency_domain_metrics(self):
        """计算频域指标"""
        # 简化的频域分析
        from scipy import signal
        
        # 计算功率谱密度
        frequencies, psd = signal.welch(self.rr_intervals, fs=4)
        
        # 定义频带
        vlf_band = (0.0033, 0.04)  # 超低频
        lf_band = (0.04, 0.15)     # 低频
        hf_band = (0.15, 0.4)      # 高频
        
        # 计算各频带功率
        vlf_power = self.calculate_band_power(frequencies, psd, vlf_band)
        lf_power = self.calculate_band_power(frequencies, psd, lf_band)
        hf_power = self.calculate_band_power(frequencies, psd, hf_band)
        
        metrics = {
            'VLF_Power': vlf_power,
            'LF_Power': lf_power,
            'HF_Power': hf_power,
            'LF_HF_Ratio': lf_power / hf_power if hf_power > 0 else 0,
            'Total_Power': vlf_power + lf_power + hf_power
        }
        
        return metrics
    
    def calculate_rmssd(self):
        """计算RMSSD"""
        import numpy as np
        
        differences = np.diff(self.rr_intervals)
        squared_differences = differences ** 2
        mean_squared_diff = np.mean(squared_differences)
        rmssd = np.sqrt(mean_squared_diff)
        
        return rmssd
    
    def calculate_pnn50(self):
        """计算pNN50"""
        import numpy as np
        
        differences = np.abs(np.diff(self.rr_intervals))
        nn50 = np.sum(differences > 50)
        pnn50 = (nn50 / len(differences)) * 100
        
        return pnn50
```

### 2. HRV临床应用
```python
# HRV临床应用
class HRVClinicalApplication:
    def __init__(self, hrv_data):
        self.hrv_data = hrv_data
    
    def assess_autonomic_function(self):
        """评估自主神经功能"""
        metrics = self.hrv_data
        
        autonomic_assessment = {
            'sympathetic_activity': self.assess_sympathetic_activity(metrics),
            'parasympathetic_activity': self.assess_parasympathetic_activity(metrics),
            'autonomic_balance': self.assess_autonomic_balance(metrics),
            'stress_level': self.assess_stress_level(metrics),
            'recovery_status': self.assess_recovery_status(metrics)
        }
        
        return autonomic_assessment
    
    def assess_sympathetic_activity(self, metrics):
        """评估交感神经活动"""
        lf_power = metrics.get('LF_Power', 0)
        lf_hf_ratio = metrics.get('LF_HF_Ratio', 1)
        
        if lf_power > 500 and lf_hf_ratio > 2:
            return 'high'  # 交感神经活动增强
        elif lf_power > 200 and lf_hf_ratio > 1:
            return 'moderate'  # 中等交感神经活动
        else:
            return 'low'  # 交感神经活动较低
    
    def assess_parasympathetic_activity(self, metrics):
        """评估副交感神经活动"""
        hf_power = metrics.get('HF_Power', 0)
        rmssd = metrics.get('RMSSD', 0)
        
        if hf_power > 300 and rmssd > 50:
            return 'high'  # 副交感神经活动增强
        elif hf_power > 100 and rmssd > 30:
            return 'moderate'  # 中等副交感神经活动
        else:
            return 'low'  # 副交感神经活动较低
    
    def assess_stress_level(self, metrics):
        """评估压力水平"""
        lf_hf_ratio = metrics.get('LF_HF_Ratio', 1)
        sdnn = metrics.get('SDNN', 50)
        
        if lf_hf_ratio > 3 or sdnn < 30:
            return 'high_stress'  # 高压力
        elif lf_hf_ratio > 2 or sdnn < 40:
            return 'moderate_stress'  # 中等压力
        else:
            return 'low_stress'  # 低压力
```

## 个人心率数据分析

### 1. 2026年3月3日心率分析
```python
# 个人心率数据示例
personal_hr_data = {
    'date': '2026-03-03',
    'resting_hr': 50,  # 平均静息心率50bpm
    'exercise_hr': {
        'average': 131,
        'max': 158,
        'min': 95,
        'duration': 80  # 分钟
    },
    'recovery_hr': {
        'immediate_post': 158,
        '1min_post': 125,
        '2min_post': 105,
        '5min_post': 75
    }
}

# 分析示例
hr_analysis = {
    'fitness_level': 'excellent',  # 静息心率50bpm，优秀水平
    'exercise_intensity': 'moderate',  # 平均心率131，中等强度
    'recovery_quality': 'excellent',  # 2分钟恢复53次，优秀
    'training_effect': 'good_aerobic_base'  # 有氧基础建设效果好
}
```

### 2. 长期心率趋势分析
```python
# 长期趋势分析示例
long_term_hr_trends = {
    'resting_hr_trend': {
        '3_month_average': 52,
        '6_month_average': 54,
        '12_month_average': 56,
        'trend': 'improving',  # 静息心率下降，健康状况改善
        'fitness_improvement': 'significant'
    },
    'exercise_hr_trend': {
        'average_exercise_hr': 128,
        'max_exercise_hr': 162,
        'hr_drift': 'minimal',  # 心率漂移小，体能稳定
        'efficiency_improvement': 'notable'
    },
    'recovery_hr_trend': {
        '1min_recovery': 35,
        '2min_recovery': 55,
        'recovery_speed': 'improving',  # 恢复速度加快
        'cardiovascular_health': 'excellent'
    }
}
```

## 心率监测设备

### 1. 设备选择
- **胸带心率带**: 最准确，适合专业训练
- **手腕光学心率**: 方便，日常使用足够
- **智能手表**: 综合功能，心率监测+其他功能
- **运动手环**: 基本心率监测，性价比高

### 2. 数据质量控制
```python
# 数据质量控制
class HRDataQualityControl:
    def __init__(self, hr_data):
        self.hr_data = hr_data
    
    def check_data_quality(self):
        """检查数据质量"""
        quality_report = {
            'missing_data': self.check_missing_data(),
            'outliers': self.detect_outliers(),
            'noise_level': self.assess_noise_level(),
            'signal_quality': self.assess_signal_quality(),
            'recommendations': self.get_quality_recommendations()
        }
        return quality_report
    
    def detect_outliers(self):
        """检测异常值"""
        import numpy as np
        
        mean_hr = np.mean(self.hr_data)
        std_hr = np.std(self.hr_data)
        
        outliers = []
        for i, hr in enumerate(self.hr_data):
            if abs(hr - mean_hr) > 3 * std_hr:  # 3个标准差之外的值
                outliers.append({'index': i, 'hr': hr, 'deviation': hr - mean_hr})
        
        return outliers
    
    def assess_signal_quality(self):
        """评估信号质量"""
        outliers = self.detect_outliers()
        missing_data = self.check_missing_data()
        
        # 简单质量评分
        quality_score = 100
        quality_score -= len(outliers) * 2
        quality_score -= missing_data * 5
        
        if quality_score >= 90:
            return 'excellent'
        elif quality_score >= 75:
            return 'good'
        elif quality_score >= 60:
            return 'acceptable'
        else:
            return 'poor'
```

## 心率数据应用

### 1. 训练优化
- **强度调整**: 根据心率调整训练强度
- **疲劳监测**: 通过静息心率监测疲劳程度
- **恢复指导**: 用心率恢复评估恢复状态
- **周期调整**: 根据心率趋势调整训练周期

### 2. 健康管理
- **心血管健康**: 长期静息心率监测
- **压力管理**: HRV监测压力水平
- **睡眠质量**: 夜间心率监测睡眠质量
- **疾病预警**: 心率异常可能提示健康问题

---

*Last Updated: 2026-03-03*
*Version: 1.0*