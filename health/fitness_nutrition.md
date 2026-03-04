# 健身营养知识

## 营养基础理论

### 1. 宏量营养素
- **碳水化合物**: 主要能量来源，尤其对运动表现至关重要
- **蛋白质**: 肌肉修复和生长的基础
- **脂肪**: 激素合成和脂溶性维生素吸收
- **水分**: 生命活动和运动表现的基础

### 2. 微量营养素
- **维生素**: 调节代谢过程，支持免疫功能
- **矿物质**: 骨骼健康、肌肉收缩和体液平衡
- **抗氧化剂**: 对抗运动产生的氧化应激

## 运动营养需求计算

### 1. 基础代谢率计算
```python
# 营养需求计算器
class NutritionCalculator:
    def __init__(self, weight, height, age, gender, activity_level):
        self.weight = weight  # kg
        self.height = height  # cm
        self.age = age
        self.gender = gender
        self.activity_level = activity_level
    
    def calculate_bmr(self, method='mifflin_st_jeor'):
        """计算基础代谢率"""
        if method == 'mifflin_st_jeor':
            if self.gender == 'male':
                bmr = 10 * self.weight + 6.25 * self.height - 5 * self.age + 5
            else:
                bmr = 10 * self.weight + 6.25 * self.height - 5 * self.age - 161
        elif method == 'harris_benedict':
            if self.gender == 'male':
                bmr = 88.362 + 13.397 * self.weight + 4.799 * self.height - 5.677 * self.age
            else:
                bmr = 447.593 + 9.247 * self.weight + 3.098 * self.height - 4.330 * self.age
        else:
            bmr = 10 * self.weight + 6.25 * self.height - 5 * self.age + 5
        
        return bmr
    
    def calculate_tdee(self):
        """计算每日总能量消耗"""
        bmr = self.calculate_bmr()
        
        activity_multipliers = {
            'sedentary': 1.2,      # 久坐不动
            'lightly_active': 1.375,  # 轻度活动
            'moderately_active': 1.55,  # 中度活动
            'very_active': 1.725,     # 高度活动
            'extra_active': 1.9       # 极度活动
        }
        
        multiplier = activity_multipliers.get(self.activity_level, 1.55)
        tdee = bmr * multiplier
        
        return tdee
    
    def calculate_macronutrient_split(self, goal='maintenance'):
        """计算宏量营养素分配"""
        tdee = self.calculate_tdee()
        
        if goal == 'weight_loss':
            calorie_target = tdee - 500  # 每日500卡路里缺口
            protein_ratio = 0.3
            carb_ratio = 0.4
            fat_ratio = 0.3
        elif goal == 'muscle_gain':
            calorie_target = tdee + 300  # 每日300卡路里盈余
            protein_ratio = 0.3
            carb_ratio = 0.45
            fat_ratio = 0.25
        else:  # maintenance
            calorie_target = tdee
            protein_ratio = 0.25
            carb_ratio = 0.45
            fat_ratio = 0.3
        
        macros = {
            'calories': calorie_target,
            'protein': {
                'grams': (calorie_target * protein_ratio) / 4,
                'percentage': protein_ratio * 100
            },
            'carbohydrates': {
                'grams': (calorie_target * carb_ratio) / 4,
                'percentage': carb_ratio * 100
            },
            'fat': {
                'grams': (calorie_target * fat_ratio) / 9,
                'percentage': fat_ratio * 100
            }
        }
        
        return macros
```

### 2. 运动营养需求
```python
# 运动营养计算器
class SportsNutritionCalculator:
    def __init__(self, athlete_profile):
        self.weight = athlete_profile['weight']
        self.activity_type = athlete_profile['activity_type']
        self.training_hours = athlete_profile['training_hours']
        self.training_intensity = athlete_profile['training_intensity']
    
    def calculate_sports_nutrition(self):
        """计算运动营养需求"""
        base_nutrition = self.calculate_base_requirements()
        training_nutrition = self.calculate_training_adjustments()
        
        total_nutrition = {
            'pre_workout': self.calculate_pre_workout_nutrition(),
            'during_workout': self.calculate_during_workout_nutrition(),
            'post_workout': self.calculate_post_workout_nutrition(),
            'daily_total': self.combine_daily_nutrition(base_nutrition, training_nutrition)
        }
        
        return total_nutrition
    
    def calculate_base_requirements(self):
        """计算基础营养需求"""
        # 运动员基础需求通常比普通人高
        protein_per_kg = 1.6  # 运动员蛋白质需求: 1.6-2.2 g/kg
        carb_per_kg = 6  # 运动员碳水需求: 5-8 g/kg
        fat_per_kg = 1.2  # 运动员脂肪需求: 1.0-1.5 g/kg
        
        base_nutrition = {
            'protein': self.weight * protein_per_kg,
            'carbohydrates': self.weight * carb_per_kg,
            'fat': self.weight * fat_per_kg,
            'calories': (self.weight * protein_per_kg * 4 + 
                        self.weight * carb_per_kg * 4 + 
                        self.weight * fat_per_kg * 9)
        }
        
        return base_nutrition
    
    def calculate_pre_workout_nutrition(self):
        """计算训练前营养"""
        # 训练前2-3小时
        pre_workout = {
            'timing': '2-3小时 before training',
            'carbohydrates': self.weight * 1.5,  # 1-2 g/kg
            'protein': self.weight * 0.3,        # 0.25-0.4 g/kg
            'fat': self.weight * 0.3,            # 适量脂肪
            'example_foods': [
                '燕麦+香蕉+牛奶',
                '全麦面包+鸡蛋+花生酱',
                '酸奶+水果+坚果'
            ]
        }
        
        return pre_workout
    
    def calculate_during_workout_nutrition(self):
        """计算训练中营养"""
        duration = self.training_hours
        
        if duration < 1:
            during_workout = {
                'needed': False,
                'reason': '训练时间短于1小时，通常不需要额外营养',
                'suggestion': '保持充足水分即可'
            }
        elif duration < 2:
            during_workout = {
                'needed': True,
                'carbohydrates': duration * 30,  # 30-60 g/hour
                'hydration': duration * 500,    # 500-1000 ml/hour
                'electrolytes': '适量补充',
                'suggestion': '运动饮料或能量胶'
            }
        else:
            during_workout = {
                'needed': True,
                'carbohydrates': duration * 60,  # 60-90 g/hour
                'hydration': duration * 750,    # 750-1000 ml/hour
                'electrolytes': '充足补充',
                'protein': '训练后可补充',
                'suggestion': '运动饮料+能量胶+香蕉'
            }
        
        return during_workout
    
    def calculate_post_workout_nutrition(self):
        """计算训练后营养"""
        # 训练后30-60分钟是黄金窗口期
        post_workout = {
            'timing': '训练后30-60分钟内',
            'carbohydrates': self.weight * 1.2,  # 1-1.2 g/kg
            'protein': self.weight * 0.4,        # 0.3-0.4 g/kg
            'ratio': '碳水:蛋白质 = 3:1 或 4:1',
            'importance': '促进肌肉恢复和糖原补充',
            'example_foods': [
                '乳清蛋白+香蕉',
                '鸡胸肉+米饭',
                '酸奶+水果+燕麦'
            ]
        }
        
        return post_workout
```

## 营养素详解

### 1. 碳水化合物
```python
# 碳水化合物营养
class CarbohydrateNutrition:
    def __init__(self):
        self.carb_types = {
            'simple_carbs': {
                'examples': ['葡萄糖', '果糖', '蔗糖', '白面包', '白米饭'],
                'digestion': '快速消化',
                'glycemic_index': '高',
                'best_timing': '训练前后'
            },
            'complex_carbs': {
                'examples': ['燕麦', '全麦面包', '糙米', '红薯', '豆类'],
                'digestion': '缓慢消化',
                'glycemic_index': '中低',
                'best_timing': '日常主食'
            },
            'fiber': {
                'examples': ['蔬菜', '水果', '全谷物', '坚果'],
                'digestion': '不被消化',
                'glycemic_index': '无',
                'benefits': '肠道健康、血糖控制'
            }
        }
    
    def calculate_carb_timing(self, training_schedule):
        """计算碳水摄入时机"""
        timing_plan = {
            'pre_training': self.get_pre_training_carb_plan(training_schedule),
            'during_training': self.get_during_training_carb_plan(training_schedule),
            'post_training': self.get_post_training_carb_plan(training_schedule),
            'rest_days': self.get_rest_day_carb_plan(training_schedule)
        }
        
        return timing_plan
    
    def get_pre_training_carb_plan(self, training):
        """训练前碳水计划"""
        intensity = training['intensity']
        duration = training['duration']
        
        if intensity == 'high' and duration > 90:
            return {
                'timing': '训练前3-4小时',
                'amount': '1-1.2 g/kg体重',
                'type': '复合碳水为主',
                'examples': ['燕麦+水果', '全麦面包+鸡蛋']
            }
        else:
            return {
                'timing': '训练前2-3小时',
                'amount': '0.8-1 g/kg体重',
                'type': '易消化碳水',
                'examples': ['香蕉', '酸奶+蜂蜜']
            }
```

### 2. 蛋白质营养
```python
# 蛋白质营养
class ProteinNutrition:
    def __init__(self):
        self.protein_sources = {
            'animal_protein': {
                'complete': True,
                'examples': ['鸡胸肉', '牛肉', '鱼肉', '鸡蛋', '牛奶'],
                'quality': '高生物价',
                'digestion': '良好'
            },
            'plant_protein': {
                'complete': False,
                'examples': ['豆类', '豆腐', '豆浆', '坚果', '全谷物'],
                'quality': '需组合食用',
                'digestion': '较慢'
            },
            'supplements': {
                'examples': ['乳清蛋白', '大豆蛋白', '酪蛋白'],
                'quality': '高质量',
                'digestion': '快速',
                'convenience': '方便'
            }
        }
    
    def calculate_protein_timing(self, training_type):
        """计算蛋白质摄入时机"""
        timing = {
            'immediate_post': {
                'timing': '训练后30分钟内',
                'amount': '20-30g',
                'purpose': '快速启动肌肉修复',
                'best_sources': ['乳清蛋白', '鸡蛋']
            },
            'meal_timing': {
                'timing': '每3-4小时一次',
                'amount': '20-40g',
                'purpose': '维持肌肉蛋白质合成',
                'distribution': '均匀分布'
            },
            'before_bed': {
                'timing': '睡前30分钟',
                'amount': '20-30g',
                'purpose': '夜间肌肉修复',
                'best_sources': ['酪蛋白', '酸奶']
            }
        }
        
        return timing
    
    def calculate_daily_protein_distribution(self, total_protein, meals=5):
        """计算每日蛋白质分配"""
        distribution = {}
        
        # 训练后蛋白质
        distribution['post_workout'] = 0.25 * total_protein
        
        # 睡前蛋白质
        distribution['before_sleep'] = 0.15 * total_protein
        
        # 剩余蛋白质平均分配到其他餐次
        remaining_protein = total_protein - distribution['post_workout'] - distribution['before_sleep']
        per_meal = remaining_protein / (meals - 2)
        
        for i in range(1, meals - 1):
            distribution[f'meal_{i}'] = per_meal
        
        return distribution
```

### 3. 脂肪营养
```python
# 脂肪营养
class FatNutrition:
    def __init__(self):
        self.fat_types = {
            'saturated_fat': {
                'examples': ['动物脂肪', '椰子油', '黄油'],
                'health_impact': '适量摄入',
                'daily_limit': '总热量的10%以内'
            },
            'monounsaturated_fat': {
                'examples': ['橄榄油', '牛油果', '坚果'],
                'health_impact': '有益心血管',
                'recommended': '主要脂肪来源'
            },
            'polyunsaturated_fat': {
                'examples': ['鱼类', '亚麻籽油', '葵花籽油'],
                'health_impact': '必需脂肪酸',
                'omega3_omega6_balance': '重要'
            },
            'trans_fat': {
                'examples': ['油炸食品', '加工食品'],
                'health_impact': '有害健康',
                'recommendation': '尽量避免'
            }
        }
    
    def calculate_omega3_requirement(self, health_goals):
        """计算Omega-3需求"""
        requirements = {
            'general_health': '250-500mg EPA+DHA',
            'cardiovascular_health': '500-1000mg EPA+DHA',
            'inflammatory_conditions': '1000-2000mg EPA+DHA',
            'athletic_performance': '1000-1500mg EPA+DHA'
        }
        
        return requirements.get(health_goals, '250-500mg EPA+DHA')
    
    def recommend_fat_sources(self, health_goals):
        """推荐脂肪来源"""
        recommendations = {
            'primary_sources': [
                '橄榄油',
                '牛油果',
                '坚果(杏仁、核桃)',
                '深海鱼类(三文鱼、沙丁鱼)'
            ],
            'cooking_oils': [
                '橄榄油(低温烹饪)',
                '椰子油(中温烹饪)',
                '牛油果油(高温烹饪)'
            ],
            'supplements': [
                '鱼油(Omega-3)',
                '维生素E(抗氧化)'
            ]
        }
        
        return recommendations
```

## 水分与电解质

### 1. 水分需求
```python
# 水分需求分析
class HydrationAnalysis:
    def __init__(self, body_weight, activity_level, climate):
        self.body_weight = body_weight
        self.activity_level = activity_level
        self.climate = climate
    
    def calculate_daily_fluid_needs(self):
        """计算每日水分需求"""
        # 基础需求: 30-35 ml/kg体重
        base_fluid = self.body_weight * 35
        
        # 活动调整
        activity_adjustment = self.calculate_activity_adjustment()
        
        # 气候调整
        climate_adjustment = self.calculate_climate_adjustment()
        
        total_fluid = base_fluid + activity_adjustment + climate_adjustment
        
        return {
            'base_fluid': base_fluid,
            'activity_adjustment': activity_adjustment,
            'climate_adjustment': climate_adjustment,
            'total_daily_fluid': total_fluid,
            'minimum_fluid': self.body_weight * 30
        }
    
    def calculate_activity_adjustment(self):
        """计算活动调整"""
        adjustment = 0
        
        if self.activity_level == 'sedentary':
            adjustment = 0
        elif self.activity_level == 'light':
            adjustment = 500  # ml
        elif self.activity_level == 'moderate':
            adjustment = 1000  # ml
        elif self.activity_level == 'active':
            adjustment = 1500  # ml
        elif self.activity_level == 'very_active':
            adjustment = 2000  # ml
        
        return adjustment
    
    def calculate_sweat_rate(self, pre_weight, post_weight, fluid_intake, duration):
        """计算出汗率"""
        weight_loss = pre_weight - post_weight  # kg
        sweat_loss = weight_loss * 1000  # ml
        total_sweat = sweat_loss + fluid_intake
        
        sweat_rate = total_sweat / duration  # ml/hour
        
        return sweat_rate
```

### 2. 电解质平衡
```python
# 电解质管理
class ElectrolyteManagement:
    def __init__(self):
        self.electrolytes = {
            'sodium': {
                'function': '体液平衡、神经传导',
                'daily_needs': '1500-2300mg',
                'sweat_loss': '400-1000mg/L',
                'sources': ['食盐', '运动饮料', '咸味零食']
            },
            'potassium': {
                'function': '肌肉收缩、心脏功能',
                'daily_needs': '3500-4700mg',
                'sweat_loss': '150-300mg/L',
                'sources': ['香蕉', '土豆', '菠菜', '鳄梨']
            },
            'magnesium': {
                'function': '肌肉功能、能量代谢',
                'daily_needs': '300-400mg',
                'sweat_loss': '20-50mg/L',
                'sources': ['坚果', '全谷物', '绿叶蔬菜']
            },
            'calcium': {
                'function': '骨骼健康、肌肉收缩',
                'daily_needs': '1000-1300mg',
                'sweat_loss': '10-30mg/L',
                'sources': ['奶制品', '豆制品', '绿叶蔬菜']
            }
        }
    
    def calculate_electrolyte_needs(self, sweat_rate, duration, climate):
        """计算电解质需求"""
        total_sweat = sweat_rate * duration
        
        electrolyte_needs = {}
        
        for electrolyte, info in self.electrolytes.items():
            sweat_loss_per_l = info['sweat_loss']
            total_loss = (total_sweat / 1000) * sweat_loss_per_l
            
            # 考虑气候因素
            if climate == 'hot_humid':
                total_loss *= 1.5
            
            electrolyte_needs[electrolyte] = {
                'total_loss': total_loss,
                'recommended_intake': total_loss * 1.2,  # 额外20%补充
                'supplement_form': self.recommend_supplement_form(electrolyte, total_loss)
            }
        
        return electrolyte_needs
```

## 运动营养时机

### 1. 营养窗口期
```python
# 营养窗口期分析
class NutritionTiming:
    def __init__(self):
        self.timing_windows = {
            'pre_workout': {
                'optimal': '训练前2-3小时',
                'minimum': '训练前1小时',
                'maximum': '训练前4小时',
                'purpose': '提供能量、防止肌肉分解'
            },
            'during_workout': {
                'short_duration': '< 60分钟: 通常不需要',
                'medium_duration': '60-120分钟: 水分+电解质',
                'long_duration': '> 120分钟: 碳水+电解质',
                'purpose': '维持能量、补充水分'
            },
            'post_workout': {
                'golden_window': '训练后30分钟内',
                'extended_window': '训练后2小时内',
                'purpose': '肌肉修复、糖原补充'
            },
            'bedtime': {
                'timing': '睡前30分钟',
                'purpose': '夜间肌肉修复、防止肌肉分解'
            }
        }
    
    def optimize_nutrition_timing(self, training_schedule):
        """优化营养时机"""
        timing_plan = {}
        
        for training in training_schedule:
            training_time = training['time']
            duration = training['duration']
            intensity = training['intensity']
            
            timing_plan[training_time] = {
                'pre_workout': self.calculate_pre_workout_timing(training_time, duration, intensity),
                'during_workout': self.calculate_during_workout_timing(duration, intensity),
                'post_workout': self.calculate_post_workout_timing(training_time, duration)
            }
        
        return timing_plan
```

### 2. 个人化营养计划
```python
# 个人化营养计划
class PersonalizedNutritionPlan:
    def __init__(self, athlete_profile):
        self.profile = athlete_profile
        self.calculator = NutritionCalculator(
            athlete_profile['weight'],
            athlete_profile['height'],
            athlete_profile['age'],
            athlete_profile['gender'],
            athlete_profile['activity_level']
        )
    
    def create_comprehensive_plan(self):
        """创建综合营养计划"""
        plan = {
            'daily_nutrition': self.create_daily_plan(),
            'training_nutrition': self.create_training_plan(),
            'supplementation': self.create_supplement_plan(),
            'hydration': self.create_hydration_plan(),
            'meal_timing': self.create_meal_timing(),
            'special_considerations': self.create_special_considerations()
        }
        
        return plan
    
    def create_daily_plan(self):
        """创建每日营养计划"""
        macros = self.calculator.calculate_macronutrient_split(self.profile['goal'])
        
        daily_plan = {
            'calorie_target': macros['calories'],
            'macronutrients': macros,
            'micronutrients': self.calculate_micronutrient_needs(),
            'fiber_target': self.calculate_fiber_needs(),
            'meal_frequency': self.recommend_meal_frequency()
        }
        
        return daily_plan
    
    def create_training_plan(self):
        """创建训练营养计划"""
        sports_calc = SportsNutritionCalculator(self.profile)
        training_nutrition = sports_calc.calculate_sports_nutrition()
        
        return training_nutrition
```

## 营养补充剂

### 1. 运动补充剂
```python
# 运动补充剂分析
class SportsSupplements:
    def __init__(self):
        self.supplement_categories = {
            'performance_enhancers': {
                'creatine': {
                    'benefits': '力量提升、肌肉增长',
                    'dosage': '3-5g/天',
                    'timing': '任意时间',
                    'evidence': '强证据支持'
                },
                'caffeine': {
                    'benefits': '耐力提升、注意力集中',
                    'dosage': '3-6mg/kg体重',
                    'timing': '训练前30-60分钟',
                    'evidence': '中等证据支持'
                },
                'beta_alanine': {
                    'benefits': '高强度耐力',
                    'dosage': '2-5g/天',
                    'timing': '分次服用',
                    'evidence': '中等证据支持'
                }
            },
            'recovery_supplements': {
                'protein_powder': {
                    'benefits': '蛋白质补充',
                    'dosage': '20-40g/次',
                    'timing': '训练后',
                    'evidence': '强证据支持'
                },
                'branched_chain_amino_acids': {
                    'benefits': '肌肉保护',
                    'dosage': '5-10g/次',
                    'timing': '训练中/后',
                    'evidence': '中等证据支持'
                },
                'glutamine': {
                    'benefits': '免疫支持',
                    'dosage': '5-10g/天',
                    'timing': '训练后',
                    'evidence': '弱证据支持'
                }
            },
            'health_supplements': {
                'omega3_fatty_acids': {
                    'benefits': '心血管健康、抗炎',
                    'dosage': '1000-2000mg/天',
                    'timing': '随餐服用',
                    'evidence': '强证据支持'
                },
                'vitamin_d': {
                    'benefits': '骨骼健康、免疫',
                    'dosage': '800-2000IU/天',
                    'timing': '随餐服用',
                    'evidence': '中等证据支持'
                },
                'magnesium': {
                    'benefits': '肌肉功能、睡眠',
                    'dosage': '300-400mg/天',
                    'timing': '睡前',
                    'evidence': '中等证据支持'
                }
            }
        }
    
    def recommend_supplements(self, goals, budget):
        """推荐补充剂"""
        recommendations = []
        
        if 'performance' in goals:
            recommendations.extend([
                {'name': 'creatine', 'priority': 'high', 'cost': 'low'},
                {'name': 'caffeine', 'priority': 'medium', 'cost': 'low'}
            ])
        
        if 'recovery' in goals:
            recommendations.extend([
                {'name': 'protein_powder', 'priority': 'high', 'cost': 'medium'},
                {'name': 'omega3', 'priority': 'medium', 'cost': 'medium'}
            ])
        
        if 'health' in goals:
            recommendations.extend([
                {'name': 'vitamin_d', 'priority': 'high', 'cost': 'low'},
                {'name': 'magnesium', 'priority': 'medium', 'cost': 'low'}
            ])
        
        # 按预算筛选
        if budget == 'low':
            recommendations = [r for r in recommendations if r['cost'] == 'low']
        elif budget == 'medium':
            recommendations = [r for r in recommendations if r['cost'] in ['low', 'medium']]
        
        return recommendations
```

### 2. 补充剂安全性
```python
# 补充剂安全性分析
class SupplementSafety:
    def __init__(self):
        self.safety_guidelines = {
            'quality_assurance': {
                'third_party_testing': '选择经过第三方测试的产品',
                'certifications': ['NSF', 'Informed-Sport', 'USP'],
                'brand_reputation': '选择知名品牌'
            },
            'dosage_safety': {
                'follow_recommendations': '遵循推荐剂量',
                'avoid_megadoses': '避免超大剂量',
                'cycle_supplements': '适当周期使用'
            },
            'potential_side_effects': {
                'individual_tolerance': '注意个体耐受性',
                'interactions': '注意药物相互作用',
                'consult_professional': '咨询专业人士'
            },
            'regulatory_compliance': {
                'check_regulations': '了解当地法规',
                'banned_substances': '避免禁用物质',
                'sport_regulations': '了解运动禁药规定'
            }
        }
    
    def assess_supplement_safety(self, supplement, individual_profile):
        """评估补充剂安全性"""
        safety_assessment = {
            'safety_level': self.determine_safety_level(supplement),
            'potential_risks': self.identify_potential_risks(supplement, individual_profile),
            'benefits_vs_risks': self.evaluate_benefits_vs_risks(supplement, individual_profile),
            'recommendations': self.get_safety_recommendations(supplement, individual_profile)
        }
        
        return safety_assessment
```

## 个人营养记录

### 2026年3月营养记录
```python
# 个人营养数据示例
personal_nutrition_march_2026 = {
    'date': '2026-03',
    'daily_stats': {
        'average_calories': 2400,
        'average_protein': 120,  # g
        'average_carbs': 300,    # g
        'average_fat': 80,       # g
        'average_water': 2500    # ml
    },
    'training_nutrition': {
        'pre_workout_meals': ['燕麦+香蕉', '全麦面包+鸡蛋'],
        'post_workout_meals': ['蛋白粉+水果', '鸡胸肉+米饭'],
        'hydration': '充足，每日2.5-3升'
    },
    'supplements': {
        'daily': ['维生素D', 'Omega-3', '蛋白粉'],
        'training_days': ['训练前咖啡']
    }
}
```

## 营养评估工具

### 1. 营养日记分析
```python
# 营养日记分析
class NutritionDiaryAnalysis:
    def __init__(self, nutrition_data):
        self.nutrition_data = nutrition_data
    
    def analyze_nutrition_patterns(self):
        """分析营养模式"""
        analysis = {
            'macronutrient_balance': self.analyze_macronutrient_balance(),
            'calorie_trends': self.analyze_calorie_trends(),
            'timing_patterns': self.analyze_timing_patterns(),
            'food_variety': self.analyze_food_variety(),
            'compliance_score': self.calculate_compliance_score()
        }
        
        return analysis
    
    def analyze_macronutrient_balance(self):
        """分析宏量营养素平衡"""
        avg_protein = self.nutrition_data['average_protein']
        avg_carbs = self.nutrition_data['average_carbs']
        avg_fat = self.nutrition_data['average_fat']
        
        total_calories = avg_protein * 4 + avg_carbs * 4 + avg_fat * 9
        
        protein_percentage = (avg_protein * 4) / total_calories * 100
        carbs_percentage = (avg_carbs * 4) / total_calories * 100
        fat_percentage = (avg_fat * 9) / total_calories * 100
        
        balance_assessment = {
            'protein_percentage': protein_percentage,
            'carbs_percentage': carbs_percentage,
            'fat_percentage': fat_percentage,
            'balance_quality': self.assess_balance_quality(protein_percentage, carbs_percentage, fat_percentage),
            'recommendations': self.get_balance_recommendations(protein_percentage, carbs_percentage, fat_percentage)
        }
        
        return balance_assessment
```

### 2. 营养优化建议
```python
# 营养优化
class NutritionOptimization:
    def __init__(self, current_nutrition, goals):
        self.current_nutrition = current_nutrition
        self.goals = goals
    
    def generate_optimization_plan(self):
        """生成优化计划"""
        plan = {
            'calorie_optimization': self.optimize_calories(),
            'macronutrient_optimization': self.optimize_macronutrients(),
            'timing_optimization': self.optimize_timing(),
            'supplement_optimization': self.optimize_supplements(),
            'implementation_steps': self.create_implementation_steps()
        }
        
        return plan
    
    def optimize_calories(self):
        """优化卡路里摄入"""
        current_calories = self.current_nutrition['average_calories']
        goal = self.goals['primary_goal']
        
        if goal == 'weight_loss':
            target_calories = current_calories - 500
        elif goal == 'muscle_gain':
            target_calories = current_calories + 300
        else:
            target_calories = current_calories
        
        return {
            'current_calories': current_calories,
            'target_calories': target_calories,
            'adjustment_needed': target_calories - current_calories,
            'implementation': self.create_calorie_adjustment_plan(current_calories, target_calories)
        }
```

---

*Last Updated: 2026-03-03*
*Version: 1.0*