# 实验3：记忆系统的内容检索

cnd地址：https://cnb.cool/cnb-gaosh/ai-course-lab-basic

## `PromptTemplate` 的作用

### 核心功能
- **模板化提示词管理**: `PromptTemplate` 是 LangChain 提供的组件，用于创建可重用的提示词模板
- **动态内容插入**: 支持通过占位符(如 `{history}`, `{input}`)动态插入变量内容

### 主要特性

#### 1. **模板定义**
- 使用 `from_template` 方法从字符串创建模板
- 支持多种占位符语法
- 可以定义复杂的提示词结构

#### 2. **变量替换**
- 自动将输入字典中的键值对映射到模板占位符
- 例如: `{"input": "你好"}` 会替换模板中的 `{input}`

#### 3. **与Memory集成**
- 可以与 `ConversationBufferMemory` 结合使用
- 自动将 `memory.load_memory_variables({})` 的结果注入到对应占位符中

#### 4. **可重用性**
- 一次定义，多次使用
- 便于维护统一的提示词格式

### 在实验中的应用
- 定义包含对话历史的标准提示词模板
- 通过 `{history}` 占位符集成 `ConversationBufferMemory` 的历史记录
- 通过 `{input}` 占位符插入当前用户消息
- 简化提示词构建过程，提高代码可维护性

## `ConversationBufferMemory` 工作原理及手动实现
在实验2中，我们通过全局变量来管理历史会话，langchain 0.3.x版本提供了内部组件ConversationBufferMemory专门用于存储和管理对话历史记录

### 工作原理

- **内存存储机制**: `ConversationBufferMemory` 内部维护一个缓冲区，按顺序存储对话历史
- **自动追加**: 每次对话交互后，自动将用户输入和AI回复追加到历史记录中
- **上下文注入**: 通过 `memory_key` 将历史记录注入到 `PromptTemplate` 的 `{history}` 占位符中
- **角色标记**: 使用 `human_prefix` 和 `ai_prefix` 区分用户和AI的发言

### 手动实现对比

如果不使用 `ConversationBufferMemory`，需要手动实现以下功能：

#### 1. **历史记录存储**
```python
# 自动方式 (ConversationBufferMemory)
memory = ConversationBufferMemory()
memory.save_context({"input": "你好"}, {"output": "你好！"})

# 手动方式
SESSION_HISTORY[session_id].append({
    "role": "user", 
    "content": "你好"
})
SESSION_HISTORY[session_id].append({
    "role": "assistant", 
    "content": "你好！"
})
```


#### 2. **历史记录加载**
```python
# 自动方式
history = memory.load_memory_variables({})["history"]

# 手动方式  
current_history = SESSION_HISTORY[session_id]
conversation_context = ""
for msg in current_history:
    if msg["role"] == "user":
        conversation_context += f"用户: {msg['content']}\n"
    else:
        conversation_context += f"助手: {msg['content']}\n"
```


#### 3. **与Prompt集成**
```python
# 自动方式 (LangChain自动处理)
prompt = PromptTemplate.from_template(
    "History:\n{history}\nUser: {input}\nAI:"
)

# 手动方式 (需要自己拼接)
full_prompt = f"History:\n{conversation_context}\nUser: {message}\nAI:"
```


!!! note
    需要注意的是langchain 在 1.x版本中弃用了 ConversationBufferMemory，需要换用其他替代  
    实验使用的langchain版本是 0.3.x

```python
"""
实验3：记忆系统的内容检索
学生需要使用 LangChain 的 ConversationBufferMemory 管理会话历史
"""
from typing import Dict

# 提示：需要导入 LangChain 相关模块
# from langchain_core.prompts import PromptTemplate
# from langchain_community.llms import Ollama
# from langchain.memory import ConversationBufferMemory (或新版本的等效导入)
# from langchain.chains import LLMChain (或新版本的等效导入)

from langchain_core.prompts import PromptTemplate
from langchain_community.llms import Ollama
from langchain.memory import ConversationBufferMemory
from langchain.chains import LLMChain

# 全局 Memory 映射：{session_id: ConversationBufferMemory 实例}
SESSION_MEMORIES: Dict[str, ConversationBufferMemory] = {} 


def chat_with_langchain_memory(message: str, session_id: str) -> dict:
    """
    使用 LangChain 的 ConversationBufferMemory 进行对话
    
    参数:
        message: 用户消息
        session_id: 会话ID
    
    返回:
        字典，包含:
        - response (str): AI回复
        - memory_variables (dict): ConversationBufferMemory 的内部变量
    
    实现要求:
        1. 使用 ConversationBufferMemory 管理每个 session 的历史
        2. 将 Memory 对象与 Ollama LLM 集成
        3. memory_variables 应包含 'history' 键
        4. 不同 session 的 Memory 必须独立
    """
    global SESSION_MEMORIES
    
    # 实现步骤建议:
    # 1. 检查 session_id 是否存在对应的 Memory，不存在则创建
    # 2. 创建 Ollama LLM 实例
    # 3. 创建 PromptTemplate（包含历史上下文）
    # 4. 创建 LLMChain，连接 Prompt、LLM 和 Memory
    # 5. 运行链并获取响应
    # 6. 返回响应和 memory_variables
    
    # 提示：
    # 1. 检查 session_id 是否存在对应的 Memory，不存在则创建
    # 2. 创建 Ollama LLM 实例
    # 3. 创建 PromptTemplate（包含历史上下文）
    # 4. 创建 LLMChain，连接 Prompt、LLM 和 Memory
    # 5. 运行链并获取响应
    # 6. 返回响应和 memory_variables
    
     # 1. 检查 session_id 是否存在对应的 Memory，不存在则创建
    if session_id not in SESSION_MEMORIES:
        SESSION_MEMORIES[session_id] = ConversationBufferMemory(
            memory_key="history",
            input_key="input",
            ai_prefix="AI",
            human_prefix="User"
        )
    
    # 获取当前会话的 Memory 实例
    memory = SESSION_MEMORIES[session_id]
    
    # 2. 创建 Ollama LLM 实例
    llm = Ollama(model="qwen3:8b")
    
    # 3. 创建 PromptTemplate（包含历史上下文）
    prompt_template = PromptTemplate.from_template(
        "You are a helpful AI assistant. Answer the user's question based on the conversation history.\n\n"
        "Conversation History:\n{history}\n\n"
        "User: {input}\n"
        "AI:"
    )
    
    # 4. 创建 LLMChain，连接 Prompt、LLM 和 Memory
    chain = LLMChain(
        llm=llm,
        prompt=prompt_template,
        memory=memory
    )
    
    # 5. 运行链并获取响应
    result = chain.invoke({"input": message})
    response = result["text"].strip()
    
    # 6. 返回响应和 memory_variables
    return {
        "response": response,
        "memory_variables": memory.load_memory_variables({})
    }


def get_memory_summary(session_id: str) -> str:
    """
    获取指定会话的完整历史记录摘要
    
    参数:
        session_id: 会话ID
    
    返回:
        格式化的历史记录字符串，如:
        "User: 你好\nAI: 你好！有什么可以帮助你的吗？\nUser: ..."
        
        如果会话不存在，返回空字符串或提示信息
    
    实现要求:
        1. 返回人类可读的格式
        2. 包含完整的用户输入和AI回复
        3. 对不存在的 session_id，返回空字符串或提示
    """
    global SESSION_MEMORIES
    
    # 实现步骤建议:
    # 1. 检查 session_id 是否存在
    # 2. 获取 Memory 的 buffer 或 chat_memory
    # 3. 格式化消息历史为可读字符串
    # 4. 返回格式化结果
    
    # 提示：
    # 1. 检查 session_id 是否存在
    # 2. 获取 Memory 的 buffer 或 chat_memory
    # 3. 格式化消息历史为可读字符串
    # 4. 返回格式化结果
    
     # 1. 检查 session_id 是否存在
    if session_id not in SESSION_MEMORIES:
        return ""
    
    # 2. 获取 Memory 的 buffer 或 chat_memory
    memory = SESSION_MEMORIES[session_id]
    memory_variables = memory.load_memory_variables({})
    
    # 3. 格式化消息历史为可读字符串
    history = memory_variables.get("history", "")
    
    # 如果历史记录为空，返回空字符串
    if not history:
        return ""
    
    # 4. 返回格式化结果
    return history


def clear_memory(session_id: str = None):
    """
    清除会话记忆（辅助函数，用于测试）
    
    参数:
        session_id: 要清除的会话ID，如果为 None 则清除所有会话
    """
    global SESSION_MEMORIES
    
    if session_id is None:
        SESSION_MEMORIES.clear()
    elif session_id in SESSION_MEMORIES:
        del SESSION_MEMORIES[session_id]


# 测试代码（可选，用于学生本地调试）
if __name__ == "__main__":
    # 清空记忆
    clear_memory()
    
    # 测试对话
    print("=== 测试 LangChain Memory ===")
    session_id = "test_session"
    
    result1 = chat_with_langchain_memory("我的电话是13800138000", session_id)
    print(f"第1次对话:")
    print(f"  Response: {result1['response'][:50]}")
    print(f"  Memory variables keys: {result1['memory_variables'].keys()}")
    
    result2 = chat_with_langchain_memory("我的邮箱是test@example.com", session_id)
    print(f"\n第2次对话:")
    print(f"  Response: {result2['response'][:50]}")
    
    # 获取历史摘要
    summary = get_memory_summary(session_id)
    print(f"\n历史摘要:\n{summary}")
    
    # 验证信息是否保存
    print(f"\n验证信息持久化:")
    print(f"  包含电话号码: {'13800138000' in summary}")
    print(f"  包含邮箱: {'test@example.com' in summary}")
```