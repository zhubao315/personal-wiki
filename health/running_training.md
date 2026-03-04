# 跑步训练计划

## 训练基础理论

### 1. 跑步生理学
- **有氧能力**: 心肺功能和氧气利用效率
- **乳酸阈值**: 从有氧到无氧的临界点
- **跑步经济性**: 能量利用效率
- **肌肉适应性**: 肌肉力量和耐力发展

### 2. 训练原则
- **超负荷原则**: 逐步增加训练强度
- **特异性原则**: 训练要针对特定目标
- **可逆性原则**: 停止训练会失去适应性
- **个体差异原则**: 因人而异的训练方案

## 训练计划设计

### 1. 基础训练计划
```python
# 跑步训练计划生成器
class RunningTrainingPlan:
    def __init__(self, fitness_level, goal_distance, weeks=16):
        self.fitness_level = fitness_level  # beginner, intermediate, advanced
        self.goal_distance = goal_distance  # 5k, 10k, half_marathon, marathon
        self.weeks = weeks
    
    def generate_plan(self):
        """生成训练计划"""
        plan = {}
        
        if self.fitness_level == 'beginner':
            plan = self.generate_beginner_plan()
        elif self.fitness_level == 'intermediate':
            plan = self.generate_intermediate_plan()
        else:
            plan = self.generate_advanced_plan()
        
        return plan
    
    def generate_intermediate_plan(self):
        """生成中级训练计划"""
        base_weeks = 4
        build_weeks = self.weeks - base_weeks - 2
        taper_weeks = 2
        
        plan = {}
        
        # 基础阶段
        for week in range(1, base_weeks + 1):
            plan[f'Week {week}'] = {
                'Monday': 'Rest or cross-training',
                'Tuesday': f'{5 + week} km easy run',
                'Wednesday': f'{6 + week} km with intervals',
                'Thursday': f'{5 + week} km tempo run',
                'Friday': 'Rest',
                'Saturday': f'{8 + week * 1.5} km long run',
                'Sunday': f'{4 + week} km recovery run'
            }
        
        # 提升阶段
        for week in range(base_weeks + 1, base_weeks + build_weeks + 1):
            long_run_distance = 8 + base_weeks * 1.5 + (week - base_weeks) * 2
            plan[f'Week {week}'] = {
                'Monday': 'Rest or cross-training',
                'Tuesday': f'{6 + week} km hill repeats',
                'Wednesday': f'{8 + week} km with speed work',
                'Thursday': f'{6 + week} km tempo run',
                'Friday': 'Rest',
                'Saturday': f'{long_run_distance} km long run',
                'Sunday': f'{5 + week} km recovery run'
            }
        
        # 减量阶段
        for week in range(self.weeks - taper_weeks + 1, self.weeks + 1):
            taper_factor = 1 - (week - (self.weeks - taper_weeks)) * 0.3
            plan[f'Week {week}'] = {
                'Monday': 'Rest',
                'Tuesday': f'{5 * taper_factor} km easy run',
                'Wednesday': f'{6 * taper_factor} km with light intervals',
                'Thursday': 'Rest',
                'Saturday': f'{8 * taper_factor} km easy run',
                'Sunday': f'{self.goal_distance * 0.6} km goal pace run' if week == self.weeks else f'{10 * taper_factor} km easy run'
            }
        
        return plan
```

### 2. 周期化训练
```python
# 周期化训练模型
class PeriodizationTraining:
    def __init__(self, training_year):
        self.training_year = training_year
    
    def macro_cycle(self):
        """大周期划分"""
        return {
            'Preparation Phase (Jan-Mar)': '基础体能和力量训练',
            'Base Phase (Apr-Jun)': '有氧基础建设',
            'Build Phase (Jul-Sep)': '速度和强度提升',
            'Peak Phase (Oct-Nov)': '最佳状态调整',
            'Competition Phase (Dec)': '比赛和测试',
            'Recovery Phase (Late Dec)': '恢复和放松'
        }
    
    def meso_cycle(self, phase):
        """中周期计划"""
        cycles = {
            'preparation': {
                'weeks': 12,
                'focus': '力量和基本耐力',
                'intensity': 'low to moderate',
                'volume': 'gradually increasing'
            },
            'base': {
                'weeks': 12,
                'focus': '有氧能力',
                'intensity': 'moderate',
                'volume': 'high'
            },
            'build': {
                'weeks': 12,
                'focus': '速度和乳酸阈值',
                'intensity': 'high',
                'volume': 'moderate'
            },
            'peak': {
                'weeks': 6,
                'focus': '比赛配速',
                'intensity': 'very high',
                'volume': 'low'
            },
            'competition': {
                'weeks': 4,
                'focus': '比赛表现',
                'intensity': 'race pace',
                'volume': 'tapered'
            },
            'recovery': {
                'weeks': 4,
                'focus': '恢复',
                'intensity': 'very low',
                'volume': 'low'
            }
        }
        return cycles.get(phase, {})
```

## 训练强度控制

### 1. 心率区间训练
```python
# 心率区间计算
class HeartRateZones:
    def __init__(self, max_hr, resting_hr):
        self.max_hr = max_hr
        self.resting_hr = resting_hr
    
    def calculate_zones(self):
        """计算心率区间"""
        hr_reserve = self.max_hr - self.resting_hr
        
        zones = {
            'Zone 1 (Recovery)': {
                'range': (self.resting_hr + hr_reserve * 0.5, self.resting_hr + hr_reserve * 0.6),
                'percentage': '50-60% HRR',
                'purpose': '恢复和基本有氧能力',
                'duration': '30-60 minutes'
            },
            'Zone 2 (Aerobic)': {
                'range': (self.resting_hr + hr_reserve * 0.6, self.resting_hr + hr_reserve * 0.7),
                'percentage': '60-70% HRR',
                'purpose': '有氧基础建设',
                'duration': '45-120 minutes'
            },
            'Zone 3 (Tempo)': {
                'range': (self.resting_hr + hr_reserve * 0.7, self.resting_hr + hr_reserve * 0.8),
                'percentage': '70-80% HRR',
                'purpose': '乳酸阈值提升',
                'duration': '20-60 minutes'
            },
            'Zone 4 (Threshold)': {
                'range': (self.resting_hr + hr_reserve * 0.8, self.resting_hr + hr_reserve * 0.9),
                'percentage': '80-90% HRR',
                'purpose': '速度和耐力',
                'duration': '10-30 minutes'
            },
            'Zone 5 (Anaerobic)': {
                'range': (self.resting_hr + hr_reserve * 0.9, self.max_hr),
                'percentage': '90-100% HRR',
                'purpose': '最大摄氧量提升',
                'duration': '1-8 minutes'
            }
        }
        return zones
    
    def get_training_recommendation(self, current_hr, zones):
        """获取训练建议"""
        for zone_name, zone_info in zones.items():
            if zone_info['range'][0] <= current_hr <= zone_info['range'][1]:
                return {
                    'zone': zone_name,
                    'status': 'good',
                    'recommendation': f'保持当前强度，这是{zone_info["purpose"]}的理想区间'
                }
        
        if current_hr < zones['Zone 1 (Recovery)']['range'][0]:
            return {
                'zone': 'Below Zone 1',
                'status': 'too_easy',
                'recommendation': '可以适当增加强度，提高训练效果'
            }
        else:
            return {
                'zone': 'Above Zone 5',
                'status': 'too_hard',
                'recommendation': '建议降低强度，避免过度训练'
            }
```

### 2. 配速训练
```python
# 配速策略
class PaceStrategy:
    def __init__(self, goal_distance, goal_time):
        self.goal_distance = goal_distance
        self.goal_time = goal_time
        self.goal_pace = self.calculate_pace(goal_distance, goal_time)
    
    def calculate_pace(self, distance, time):
        """计算配速"""
        total_seconds = time * 60  # 转换为秒
        pace_seconds = total_seconds / distance
        minutes = int(pace_seconds // 60)
        seconds = int(pace_seconds % 60)
        return f"{minutes}:{seconds:02d}"
    
    def get_training_paces(self):
        """获取训练配速"""
        goal_seconds = self.time_to_seconds(self.goal_pace)
        
        paces = {
            'Easy Run': self.seconds_to_time(goal_seconds * 1.3),
            'Long Run': self.seconds_to_time(goal_seconds * 1.2),
            'Tempo Run': self.seconds_to_time(goal_seconds * 0.9),
            'Threshold Run': self.seconds_to_time(goal_seconds * 0.85),
            'Interval Run': self.seconds_to_time(goal_seconds * 0.75),
            'Recovery Run': self.seconds_to_time(goal_seconds * 1.5)
        }
        
        return paces
    
    def time_to_seconds(self, time_str):
        """时间字符串转秒数"""
        parts = time_str.split(':')
        if len(parts) == 2:
            return int(parts[0]) * 60 + int(parts[1])
        else:
            return int(parts[0]) * 3600
    
    def seconds_to_time(self, seconds):
        """秒数转时间字符串"""
        minutes = int(seconds // 60)
        secs = int(seconds % 60)
        return f"{minutes}:{secs:02d}"
```

## 训练方法

### 1. 间歇训练
```python
# 间歇训练计划
class IntervalTraining:
    def __init__(self, fitness_level):
        self.fitness_level = fitness_level
    
    def get_interval_workouts(self):
        """获取间歇训练方案"""
        if self.fitness_level == 'beginner':
            return self.beginner_intervals()
        elif self.fitness_level == 'intermediate':
            return self.intermediate_intervals()
        else:
            return self.advanced_intervals()
    
    def intermediate_intervals(self):
        """中级间歇训练"""
        return {
            '5K Prep': {
                'warm_up': '15分钟轻松跑',
                'intervals': '6-8 x 800m @ 5K配速',
                'rest': '90秒慢跑恢复',
                'cool_down': '10分钟轻松跑'
            },
            '10K Prep': {
                'warm_up': '20分钟轻松跑',
                'intervals': '5-6 x 1000m @ 10K配速',
                'rest': '2分钟慢跑恢复',
                'cool_down': '10分钟轻松跑'
            },
            'Speed Workout': {
                'warm_up': '15分钟轻松跑 + 动态拉伸',
                'intervals': '8-10 x 400m @ 5K配速或更快',
                'rest': '60秒完全恢复',
                'cool_down': '10分钟轻松跑'
            },
            'Tempo Intervals': {
                'warm_up': '15分钟轻松跑',
                'intervals': '3-4 x 1200m @ 乳酸阈值配速',
                'rest': '90秒慢跑恢复',
                'cool_down': '10分钟轻松跑'
            }
        }
```

### 2. 长距离训练
```python
# 长距离训练策略
class LongDistanceTraining:
    def __init__(self, goal_distance):
        self.goal_distance = goal_distance
    
    def get_long_run_strategy(self):
        """获取长跑策略"""
        long_run_distance = self.calculate_long_run_distance()
        
        strategy = {
            'Distance': f'{long_run_distance} km',
            'Pace': self.get_long_run_pace(),
            'Duration': self.calculate_duration(long_run_distance),
            'Fueling': self.get_fueling_strategy(long_run_distance),
            'Hydration': self.get_hydration_strategy(long_run_distance),
            'Pacing': self.get_pacing_strategy(),
            'Mental': self.get_mental_strategy()
        }
        return strategy
    
    def calculate_long_run_distance(self):
        """计算长跑距离"""
        if self.goal_distance == '5k':
            return 10  # 最长10公里
        elif self.goal_distance == '10k':
            return 16  # 最长16公里
        elif self.goal_distance == 'half_marathon':
            return 21  # 最长21公里
        else:  # marathon
            return 32  # 最长32公里
    
    def get_long_run_pace(self):
        """获取长跑配速"""
        if self.goal_distance == '5k':
            return '比目标配速慢1-1.5分钟/公里'
        elif self.goal_distance == '10k':
            return '比目标配速慢45-60秒/公里'
        else:
            return '比目标配速慢30-45秒/公里'
```

## 恢复与营养

### 1. 恢复策略
```python
# 恢复管理
class RecoveryManagement:
    def __init__(self):
        self.recovery_methods = {
            'active_recovery': '低强度活动促进血液循环',
            'passive_recovery': '完全休息',
            'sleep': '充足的睡眠',
            'nutrition': '适当的营养补充',
            'hydration': '充分的水分补充',
            'massage': '按摩放松',
            'stretching': '拉伸和柔韧性训练'
        }
    
    def get_recovery_plan(self, training_intensity, training_volume):
        """获取恢复计划"""
        recovery_score = self.calculate_recovery_need(training_intensity, training_volume)
        
        plan = {
            'immediate_recovery': self.get_immediate_recovery(recovery_score),
            'short_term_recovery': self.get_short_term_recovery(recovery_score),
            'long_term_recovery': self.get_long_term_recovery(recovery_score),
            'recovery_monitoring': self.get_recovery_monitoring(recovery_score)
        }
        return plan
    
    def calculate_recovery_need(self, intensity, volume):
        """计算恢复需求"""
        # 简单的恢复需求评分
        intensity_score = {'low': 1, 'moderate': 2, 'high': 3}.get(intensity, 2)
        volume_score = {'low': 1, 'moderate': 2, 'high': 3}.get(volume, 2)
        
        return intensity_score * volume_score
```

### 2. 营养计划
```python
# 跑步营养计划
class RunningNutrition:
    def __init__(self, body_weight, training_intensity):
        self.body_weight = body_weight
        self.training_intensity = training_intensity
    
    def get_daily_nutrition(self):
        """获取每日营养计划"""
        return {
            'Calories': self.calculate_daily_calories(),
            'Protein': self.calculate_protein_needs(),
            'Carbohydrates': self.calculate_carb_needs(),
            'Fats': self.calculate_fat_needs(),
            'Hydration': self.calculate_fluid_needs(),
            'Micronutrients': self.get_micronutrient_recommendations()
        }
    
    def calculate_daily_calories(self):
        """计算每日热量需求"""
        # 基础代谢率 (简化版)
        bmr = 10 * self.body_weight + 6.25 * 175 - 5 * 30 + 5  # 假设身高175cm, 年龄30岁, 男性
        
        # 活动系数
        activity_multiplier = {'low': 1.375, 'moderate': 1.55, 'high': 1.725}.get(self.training_intensity, 1.55)
        
        return bmr * activity_multiplier
    
    def calculate_protein_needs(self):
        """计算蛋白质需求"""
        # 运动员蛋白质需求: 1.2-2.0 g/kg
        protein_per_kg = {'low': 1.2, 'moderate': 1.5, 'high': 1.8}.get(self.training_intensity, 1.5)
        return self.body_weight * protein_per_kg
    
    def calculate_carb_needs(self):
        """计算碳水化合物需求"""
        # 跑步者碳水需求: 5-8 g/kg
        carb_per_kg = {'low': 5, 'moderate': 6, 'high': 7}.get(self.training_intensity, 6)
        return self.body_weight * carb_per_kg
```

## 训练监测与调整

### 1. 训练数据监测
```python
# 训练监测
class TrainingMonitoring:
    def __init__(self):
        self.metrics = [
            'distance',
            'duration',
            'pace',
            'heart_rate',
            'cadence',
            'elevation_gain',
            'training_load',
            'recovery_time'
        ]
    
    def analyze_training_data(self, training_sessions):
        """分析训练数据"""
        analysis = {
            'weekly_summary': self.calculate_weekly_summary(training_sessions),
            'trend_analysis': self.analyze_training_trends(training_sessions),
            'performance_metrics': self.calculate_performance_metrics(training_sessions),
            'recovery_analysis': self.analyze_recovery(training_sessions),
            'training_balance': self.analyze_training_balance(training_sessions)
        }
        return analysis
    
    def calculate_weekly_summary(self, sessions):
        """计算周度总结"""
        df = pd.DataFrame(sessions)
        
        summary = {
            'total_distance': df['distance'].sum(),
            'total_duration': df['duration'].sum(),
            'average_pace': df['pace'].mean(),
            'average_heart_rate': df['heart_rate'].mean(),
            'total_elevation': df['elevation_gain'].sum(),
            'training_sessions': len(df)
        }
        return summary
```

### 2. 训练调整
```python
# 训练调整策略
class TrainingAdjustment:
    def __init__(self):
        self.adjustment_triggers = {
            'overtraining': ['持续疲劳', '表现下降', '睡眠质量差', '静息心率升高'],
            'undertraining': ['缺乏挑战性', '恢复过快', '表现持续提升', '训练欲望强'],
            'plateau': ['表现停滞', '训练效果不明显', '进步缓慢']
        }
    
    def analyze_training_needs(self, training_data, performance_data):
        """分析训练需求"""
        needs = {
            'intensity_adjustment': self.analyze_intensity_needs(training_data),
            'volume_adjustment': self.analyze_volume_needs(training_data),
            'recovery_needs': self.analyze_recovery_needs(training_data),
            'technique_focus': self.analyze_technique_needs(performance_data)
        }
        return needs
    
    def recommend_adjustments(self, analysis):
        """推荐调整方案"""
        recommendations = []
        
        if analysis['intensity_needs']['needs_adjustment']:
            recommendations.append(f"强度调整: {analysis['intensity_needs']['recommendation']}")
        
        if analysis['volume_needs']['needs_adjustment']:
            recommendations.append(f"训练量调整: {analysis['volume_needs']['recommendation']}")
        
        if analysis['recovery_needs']['needs_more_recovery']:
            recommendations.append(f"恢复需求: {analysis['recovery_needs']['recommendation']}")
        
        return recommendations
```

## 伤病预防

### 1. 常见跑步伤病
- **跑步膝**: 髌股疼痛综合征
- **跟腱炎**: 跟腱炎症
- **足底筋膜炎**: 足底筋膜炎症
- **应力性骨折**: 骨骼过度使用损伤
- **肌肉拉伤**: 肌肉纤维撕裂

### 2. 预防措施
```python
# 伤病预防
class InjuryPrevention:
    def __init__(self):
        self.prevention_strategies = {
            'gradual_progression': '循序渐进增加训练量',
            'proper_footwear': '合适的跑鞋',
            'strength_training': '力量训练',
            'flexibility': '柔韧性训练',
            'rest_days': '充分的休息日',
            'cross_training': '交叉训练',
            'surface_variety': '多样化训练场地',
            'technique_focus': '技术动作改进'
        }
    
    def get_injury_prevention_plan(self, runner_profile):
        """获取伤病预防计划"""
        plan = {
            'training_modifications': self.get_training_modifications(runner_profile),
            'strength_exercises': self.get_strength_exercises(runner_profile),
            'flexibility_routine': self.get_flexibility_routine(runner_profile),
            'equipment_recommendations': self.get_equipment_recommendations(runner_profile),
            'recovery_strategies': self.get_recovery_strategies(runner_profile)
        }
        return plan
```

## 个人训练记录

### 2026年3月3日训练记录
- **训练类型**: 什刹海跑步
- **距离**: 13.14 km
- **平均心率**: 131 bpm
- **训练时长**: 80分钟
- **配速**: 6:08/km
- **天气**: 雾蒙蒙，烟雨江南感觉
- **精神状态**: 良好
- **训练感受**: 观察到各种鸭子，通海小闸口被黑色白嘴鸭占领，迎春花开了满山黄

### 训练总结
- **优点**: 坚持早起训练，享受自然风光
- **改进**: 需要更系统的训练计划
- **目标**: 提升10K和半马成绩
- **计划**: 制定16周训练计划

---

*Last Updated: 2026-03-03*
*Version: 1.0*