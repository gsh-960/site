# 实验1：结构化提示词与输出
cnd地址：https://cnb.cool/cnb-gaosh/ai-course-lab-basic

## Ollama API 调用参数说明

### 核心参数配置

- **`url`**: `"http://localhost:11434/api/generate"`
  - Ollama 本地服务的 API 端点地址
  - 默认端口为 `11434`

- **`payload`**: 请求载荷对象
  - **`model`**: `"qwen3:8b"`
    - 指定使用的模型名称和版本
    - `qwen3` 表示通义千问第三代模型
    - `8b` 表示模型参数量为 80 亿
  - **`prompt`**: 结构化提示词内容
    - 包含角色设定、任务描述和输出格式要求
  - **`stream`**: `False`
    - 控制是否启用流式输出
    - `False` 表示等待完整响应后一次性返回

### 使用前提条件

- 需要本地安装并运行 Ollama 服务
- 目标模型 `qwen3:8b` 需要预先下载到本地
- 确保端口 `11434` 未被其他应用占用

```python
"""
实验1：结构化提示词与输出
学生需要实现 classify_text 函数，使用 Pydantic 模型返回结构化的文本分类结果
"""
from typing import List
from pydantic import BaseModel, Field
import httpx
import json


class TextClassification(BaseModel):
    """
    文本分类结果的数据模型
    """
    category: str = Field(
        ..., 
        description="文本分类类别，必须是以下之一：'新闻', '技术', '体育', '娱乐', '财经'"
    )
    confidence_score: float = Field(
        ..., 
        ge=0.0, 
        le=1.0,
        description="分类置信度，范围0.0-1.0"
    )
    keywords: List[str] = Field(
        ..., 
        min_length=1, 
        max_length=5,
        description="从文本中提取的1-5个关键词"
    )


def classify_text(text: str) -> TextClassification:
    """
    对输入文本进行分类，返回结构化的分类结果
    """
    # 构建结构化 Prompt，为模型定义明确角色
    prompt = f"""
    角色：你是一个专业的文本分类专家，擅长分析文本内容并将其准确归类。
    任务：对给定文本进行精确分类，提取关键词，并评估分类置信度

    分类类别（必须从中选择）：
    - 新闻：时事、社会、政治等新闻报道
    - 技术：科技、互联网、软件、硬件等技术内容
    - 体育：各类体育赛事、运动员相关消息
    - 娱乐：影视、音乐、明星、游戏等娱乐内容
    - 财经：股市、经济、金融、商业等财经信息

    文本内容："{text}"

    请严格按照以下JSON格式输出结果，不要添加任何解释文字：
    {{
        "category": "从上述5个类别中选择最匹配的一个",
        "confidence_score": 0.0到1.0之间的数值，表示分类的置信度,
        "keywords": ["关键词1", "关键词2", "关键词3"]  // 提取1-5个最能代表文本主题的关键词
    }}

    输出要求：
    1. 仅输出有效的JSON，不包含其他任何文字
    2. category必须严格匹配预定义的5个类别之一
    3. confidence_score必须是0.0-1.0范围内的浮点数
    4. keywords数组必须包含1-5个字符串元素
    """

    # 调用 Ollama API
    url = "http://localhost:11434/api/generate"
    payload = {
        "model": "qwen3:8b",
        "prompt": prompt,
        "stream": False
    }
    
    try:
        response = httpx.post(url, json=payload, timeout=30.0)
        response.raise_for_status()
        
        # 解析响应中的 JSON 字符串
        result = response.json()
        output_text = result["response"].strip()
        
        # 提取JSON部分
        start_idx = output_text.find('{')
        end_idx = output_text.rfind('}') + 1
        
        if start_idx != -1 and end_idx > start_idx:
            json_str = output_text[start_idx:end_idx]
            data = json.loads(json_str)
            
            # 使用 TextClassification.model_validate() 创建实例
            classification = TextClassification.model_validate(data)
            
            # 返回验证后的 Pydantic 模型实例
            return classification
        else:
            raise ValueError("无法从模型输出中提取有效的JSON")
            
    except httpx.RequestError as e:
        raise Exception(f"API请求失败: {e}")
    except json.JSONDecodeError as e:
        raise Exception(f"JSON解析失败: {e}")
    except Exception as e:
        raise Exception(f"分类过程出错: {e}")



# 测试代码（可选，用于学生本地调试）
if __name__ == "__main__":
    # 测试示例
    test_texts = [
        "OpenAI发布GPT-5，性能提升10倍",
        "中国队在巴黎奥运会夺得金牌",
        "A股市场今日大涨，沪指突破3000点"
    ]
    
    for text in test_texts:
        try:
            result = classify_text(text)
            print(f"\n文本: {text}")
            print(f"分类: {result.category}")
            print(f"置信度: {result.confidence_score}")
            print(f"关键词: {result.keywords}")
        except Exception as e:
            print(f"错误: {e}")

```