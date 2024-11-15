<template>
    <div class="console-input">
        <h2 class="title">JavaScript In-line actuators</h2>

        <!-- 输入框 -->
        <textarea v-model="code" ref="codeInput" placeholder="Type JavaScript code..." rows="10" cols="50"></textarea>

        <!-- 执行按钮 -->
        <button @click="executeCode" class="execute-btn">Executable Code</button>

        <!-- 显示执行结果 -->
        <div v-if="output" class="output">
            <h3>Results:</h3>
            <pre>{{ output }}</pre> <!-- 显示输出 -->
        </div>
    </div>
</template>

<script>
import Prism from 'prismjs';
import 'prismjs/themes/prism.css'; // 默认主题
import 'prismjs/components/prism-javascript.min.js'; // 支持 JavaScript 高亮

export default {
    data() {
        return {
            code: '', // 用户输入的 JavaScript 代码
            output: '', // 执行输出
        };
    },
    methods: {
        executeCode() {
            this.output = ''; // 清空之前的输出
            try {
                const userCode = this.code;
                const result = this.runScriptInSandbox(userCode);
                if (result !== undefined) {
                    this.output = result;
                } else {
                    this.output = '无输出'; // 如果没有返回值，显示 '无输出'
                }
            } catch (error) {
                this.output = `代码执行出错: ${error.message}`;
            }
        },

        runScriptInSandbox(jsCode) {
            let output = '';
            const sandbox = {
                console: {
                    log: (msg) => {
                        output += msg + '\n'; // 捕获 console.log 输出
                    },
                },
            };

            try {
                if (jsCode) {
                    new Function('with(this) { ' + jsCode + '}').call(sandbox);
                }
            } catch (e) {
                output = `执行错误：${e.message}`;
            }

            return output;
        },
    },
};
</script>

<style scoped>
/* 整体布局和背景 */
.console-input {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
    color: #fff;
    padding: 20px;
}

/* 标题 */
.title {
    font-size: 32px;
    font-weight: bold;
    margin-bottom: 20px;
    text-align: center;
    user-select: none;
}

/* 输入框 */
textarea {
    padding: 15px;
    font-family: 'Courier New', Courier, monospace;
    font-size: 16px;
    width: 100%;
    max-width: 700px;
    min-height: 200px;
    background-color: #1e1e1e;
    color: #dcdcdc;
    border: 1px solid #444;
    border-radius: 8px;
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.3);
    resize: none;
    transition: all 0.3s ease;
}

textarea:focus {
    outline: none;
    border-color: #2575fc;
    box-shadow: 0 0 8px rgba(37, 117, 252, 0.7);
}

/* 按钮 */
.execute-btn {
    padding: 12px 24px;
    font-size: 18px;
    background-color: #2575fc;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    margin-top: 20px;
    transition: all 0.3s ease;
    user-select: none;
}

.execute-btn:hover {
    background-color: #1e62d5;
}

.execute-btn:focus {
    outline: none;
}

/* 输出区域 */
.output {
    background-color: #222;
    border-radius: 8px;
    padding: 15px;
    width: 100%;
    max-width: 700px;
    margin-top: 20px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.4);
    color: #f5f5f5;
}

.output h3{
    user-select: none;
}

pre {
    white-space: pre-wrap;
    word-wrap: break-word;
    font-size: 16px;
    color: #b8b8b8;
}

/* 响应式设计：适配小屏幕 */
@media (max-width: 768px) {
    .console-input {
        padding: 10px;
    }

    .title {
        font-size: 28px;
    }

    textarea {
        font-size: 14px;
        width: 100%;
        max-width: 100%;
    }

    .execute-btn {
        font-size: 16px;
        padding: 10px 20px;
    }

    .output {
        max-width: 100%;
    }
}
</style>
