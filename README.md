# EmbedAI - STM32 Code Analysis Platform

![License](https://img.shields.io/badge/license-Academic-blue)
![Python](https://img.shields.io/badge/python-3.8+-green)
![VS Code](https://img.shields.io/badge/VS%20Code-Extension-orange)

## 📋 Overview

**EmbedAI** is an academic project designed to facilitate the development and testing of STM32 code through AI-powered tools. This intelligent platform assists developers in writing, analyzing, and improving STM32 code with advanced machine learning capabilities.

### 🎯 Project Components

The project consists of two main components:

1. **Web Application** - A platform for intelligent code analysis and learning tools
2. **VS Code Extension** - An integrated development environment enhancement for STM32 developers

---

## ✨ Features

### 🌐 Web Application

The web application allows users to either **write STM32 code manually** or **upload existing STM32 code files**. Once submitted, the code is analyzed using multiple tools:

#### Static Analysis
- **Clang Compilation** - Detects syntax errors and compilation issues
- **Clang-Tidy** - Advanced static analysis for code quality and potential bugs
- **GCC Support** - Compatibility with ARM GCC toolchain for STM32

#### AI-Powered Analysis
- **Bi-LSTM Bug Detection** - Classifies code snippets as `buggy` or `clean`
- **Error Type Classification** - Identifies specific error types (MLP-based)
- **Line-by-Line Analysis** - Detailed error detection per code line

#### User Features
- **Download PDF Reports** - Comprehensive analysis reports with suggestions
- **Statistical Insights** - Error type frequency and distribution analytics
- **History Tracking** - Review past analysis sessions for learning

---

### 🧩 VS Code Extension (stm32-buddy)

The VS Code extension brings intelligent development features directly into the coding environment:

#### AI-Powered Features
- **Prompt-to-Code Generation** - Natural language to STM32 code translation using fine-tuned CodeT5
- **Automatic Comment Generation** - One-click descriptive comments via T5 model
- **Smart Autocompletion** - GPT-2 based STM32-specific code suggestions
- **Bug Detection** - Real-time buggy/clean classification with Bi-LSTM
- **Unit Test Generation** - Automatic STM32-specific unit tests

#### Developer Tools
- **Integrated Chatbot** - STM32 technical Q&A support
- **Error Type Prediction** - Identifies specific error categories
- **Code Analysis** - Detailed feedback on error location and type

---

## 🛠 Tech Stack

### Frontend (Web App)
- **HTML5** - Structure
- **CSS3** - Styling
- **JavaScript** - Interactivity
- **jQuery** - DOM manipulation
- **AJAX** - Asynchronous requests

### Backend
- **FastAPI** - Python web framework
- **Clang** - LLVM-based compiler for STM32 code analysis
- **Clang-Tidy** - Static analysis tool
- **GCC** - ARM GCC toolchain support

### AI/ML Models
- **T5 (Text-to-Text Transfer Transformer)** - Comment generation and code generation
- **Bi-LSTM (Bidirectional LSTM)** - Bug/clean classification
- **MLP (Multi-Layer Perceptron)** - Error type classification
- **GPT-2** - STM32 autocompletion
- **CodeT5 with PEFT (Parameter-Efficient Fine-Tuning)** - Prompt-to-code generation

### VS Code Extension
- **TypeScript** - Extension development
- **VS Code API** - Integration with VS Code
- **Node.js** - Runtime environment

### Tools & Libraries
- **PyTorch** - Deep learning framework
- **TensorFlow/Keras** - Alternative ML framework
- **Transformers (Hugging Face)** - Pre-trained models
- **scikit-learn** - ML utilities
- **Unity & CMock** - Unit testing framework

---

## 📁 Project Structure

```
EmbedAI-main/
├── Multitask/                    # Backend API with multiple AI models
│   ├── app.py                    # FastAPI application with all endpoints
│   ├── AI/
│   │   ├── modeles/              # T5 model files for comment generation
│   │   └── run_comment.py        # Comment generation script
│   ├── bug_classifier.pth        # Bi-LSTM model for bug detection
│   ├── model.h5                  # Keras model for error classification
│   ├── label_encoder.joblib      # Label encoder for error types
│   ├── vectorizer.joblib         # Text vectorizer
│   ├── vocab.json                # Vocabulary for Bi-LSTM
│   ├── symbol_table.json         # STM32 symbol table
│   └── requirements.txt          # Python dependencies
│
├── PromptToCode/                 # Prompt-to-code generation service
│   ├── server.py                 # FastAPI server for code generation
│   ├── adapter_model.safetensors # Fine-tuned CodeT5 adapter
│   ├── adapter_config.json       # Adapter configuration
│   ├── vocab.json                # Tokenizer vocabulary
│   └── merges.txt                # BPE merges
│
├── stm32-buddy/                  # VS Code extension
│   ├── src/
│   │   ├── extension.ts          # Main extension logic
│   │   └── test/                 # Extension tests
│   ├── package.json              # Node.js dependencies
│   ├── tsconfig.json             # TypeScript configuration
│   └── README.md                 # Extension documentation
│
├── Code Comment Generation/      # Comment generation notebooks
│   └── Code_Comments_Generation.ipynb
│
├── Test-Generation/              # Unit test generation
│   └── test_generation.ipynb
│
├── fine tune pi/                 # Autocompletion model training
│   └── stm32_autocompletion_notebook.ipynb
│
├── prompt_to_code/               # Prompt-to-code training
│   ├── iyadh_ghiloufi_prompt_to_code.ipynb
│   └── PTC_model/
│
├── biLSTM.ipynb                  # Bi-LSTM training notebook
├── README.md                     # This file
└── .gitignore                    # Git ignore rules
```

---

## 🚀 Installation

### Prerequisites

- Python 3.8 or higher
- Node.js (for VS Code extension)
- Clang compiler
- Git

### Backend Setup

1. **Clone the repository**
```bash
git clone https://github.com/your-username/EmbedAI.git
cd EmbedAI
```

2. **Install Python dependencies**
```bash
cd Multitask
pip install -r requirements.txt
```

3. **Install Clang and Clang-Tidy**
```bash
# Ubuntu/Debian
sudo apt-get install clang clang-tidy

# macOS
brew install llvm

# Windows
# Download from https://releases.llvm.org/
```

4. **Run the FastAPI server**
```bash
cd Multitask
uvicorn app:app --host 0.0.0.0 --port 8000
```

### VS Code Extension Setup

1. **Navigate to extension directory**
```bash
cd stm32-buddy
```

2. **Install dependencies**
```bash
npm install
```

3. **Build the extension**
```bash
npm run compile
```

4. **Package the extension**
```bash
npm run package
```

5. **Install in VS Code**
   - Open VS Code
   - Go to Extensions → Install from VSIX
   - Select the generated `.vsix` file

---

## 📖 Usage

### API Endpoints

The FastAPI backend provides the following endpoints:

#### Bug Detection
```bash
POST /predict
Content-Type: application/json

{
  "code": "int main() { int a; printf(\"%d\", a); }"
}

Response:
{
  "prediction": "buggy"  // or "clean"
}
```

#### Comment Generation
```bash
POST /generate-comment
Content-Type: application/json

{
  "code": "void setup() { pinMode(13, OUTPUT); }"
}

Response:
{
  "commented_code": "void setup() { pinMode(13, OUTPUT); // Set pin 13 as output }"
}
```

#### Error Type Prediction
```bash
POST /predict-error-type
Content-Type: application/json

{
  "code": "int a = 5 / 0;"
}

Response:
{
  "error_type": "division by zero"
}
```

#### Line-by-Line Error Analysis
```bash
POST /predict-lines-error-type
Content-Type: application/json

{
  "code": "int a;\nint b = a + 5;"
}

Response:
{
  "errors": [
    {
      "line": 1,
      "error_type": "uninitialized variable",
      "code": "int a;"
    }
  ]
}
```

#### STM32 Autocompletion
```bash
POST /stm32-autocomplete
Content-Type: application/json

{
  "prefix": "HAL_GPIO_"
}

Response:
{
  "completion": "WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_SET)"
}
```

#### Prompt-to-Code Generation
```bash
POST /generate
Content-Type: application/json

{
  "prompt": "blink LED on pin 13"
}

Response:
{
  "code": "void setup() {\n  pinMode(13, OUTPUT);\n}\n\nvoid loop() {\n  digitalWrite(13, HIGH);\n  delay(1000);\n  digitalWrite(13, LOW);\n  delay(1000);\n}"
}
```

### VS Code Extension Commands

- **EmbedAI: Generate Code** - Generate STM32 code from natural language prompt
- **EmbedAI: Generate Comments** - Add comments to selected code
- **EmbedAI: Detect Bug** - Check if selected code is buggy
- **EmbedAI: Generate Tests** - Generate unit tests for selected code
- **EmbedAI: Autocomplete** - Get STM32-specific code suggestions

---

## 🧪 Model Training

### Bi-LSTM Bug Classifier

Training notebook: `biLSTM.ipynb`

Dataset: DetectVul/devign (Hugging Face)

Architecture:
- Embedding layer (64 dimensions)
- Bidirectional LSTM (128 hidden units)
- Sigmoid output layer

### T5 Comment Generator

Training notebook: `Code Comment Generation/Code_Comments_Generation.ipynb`

Model: T5-base fine-tuned on STM32 code comments

### GPT-2 Autocompletion

Training notebook: `fine tune pi/stm32_autocompletion_notebook.ipynb`

Model: GPT-2 fine-tuned on STM32 code snippets

### Prompt-to-Code

Training notebook: `prompt_to_code/iyadh_ghiloufi_prompt_to_code.ipynb`

Model: CodeT5 with PEFT adapter fine-tuned on prompt-code pairs

---

## 🤝 Contributing

This is an academic project. For contributions:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 License

This project is part of an academic PFE (Projet de Fin d'Études) with Actia.

---

## 👨‍💻 Authors

- Project developed as part of PFE Actia
- Academic project for STM32 development assistance

---

## 🙏 Acknowledgments

- Hugging Face for pre-trained models
- TensorFlow and PyTorch communities
- STM32 community for code examples
- Clang/LLVM team for static analysis tools

---

## 📞 Support

For questions or issues, please open an issue on GitHub or contact the project maintainers.
