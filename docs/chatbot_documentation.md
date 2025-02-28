# AI Assistant: Complete Documentation

## Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Web Interface](#web-interface)
- [API Reference](#api-reference)
- [Tools & Capabilities](#tools--capabilities)
- [System Architecture](#system-architecture)
- [Installation Guide](#installation-guide)
- [Security Considerations](#security-considerations)
- [Developer Notes](#developer-notes)

## Overview

This AI Assistant integrates powerful language models with a comprehensive suite of tools to help accomplish real-world tasks through natural language. Unlike typical chatbots, this assistant can access and manipulate your local environment, search the web, generate images, manage files, and much more.

The system provides:
- **Conversational AI**: Natural language interaction with an AI model
- **Tool Integration**: Seamless use of various tools through conversation
- **Web Interface**: Clean, modern web UI for human interaction
- **REST API**: Comprehensive API for integration with other applications
- **File System Access**: Safe, controlled access to your local files
- **Markdown Support**: Rich text formatting in responses

## Features

### Information Retrieval
- **Internet Search**: Search the web for information
- **Web Page Reading**: Extract and summarize content from web pages
- **Background Checks**: Gather publicly available information about individuals
- **YouTube Transcription**: Extract and summarize YouTube video content

### Multimedia
- **Image Generation**: Create images from text descriptions using Stable Diffusion
- **Google Image Search**: Find and download relevant images
- **Image Management**: List, view, and organize images
- **Voice Input/Output**: Convert speech to text and text to speech

### System Utilities
- **Date and Time**: Get current date/time information
- **Weather Data**: Retrieve current weather and forecasts
- **PDF Analysis**: Extract and analyze text from PDF files
- **File Management**: List, move, copy, download, and delete files

## Web Interface

The web interface is available at `http://localhost:8000` and provides:

- **Chat Interface**: Send messages and view AI responses
- **Tool Access**: Buttons for quick access to common tools
- **Image Display**: View generated and downloaded images
- **Voice Controls**: Speak to your assistant and hear responses

### Example Use Cases

#### Research Assistant
```
You: I need to research quantum computing for a paper. Can you help?
Assistant: I'd be happy to help with your quantum computing research. Let me search for some information...

*Assistant provides search results, summaries from academic sources, and offers to save key information to a file*
```

#### Creative Partner
```
You: I need a logo for my bakery called "Sweet Mornings"
Assistant: I'll help create a logo concept for "Sweet Mornings" bakery.

*Assistant generates images of logo concepts, which you can refine through conversation*
```

#### Productivity Helper
```
You: I need to organize my Downloads folder
Assistant: I can help with that. Let me check what's in your Downloads folder.

*Assistant lists files, suggests categories, and helps move them to appropriate locations*
```

## API Reference

### Chat and Conversation

| Endpoint | Method | Description | Parameters |
|----------|--------|-------------|------------|
| `/chat` | POST | Single-turn chat | `content`: Text message<br>`markdown`: Boolean to enable markdown |
| `/conversation` | POST | Multi-turn conversation | Array of message objects with `role` and `content` fields |

### Tools

| Endpoint | Method | Description | Parameters |
|----------|--------|-------------|------------|
| `/tool` | POST | Generic tool caller | `tool`: Tool name<br>`args`: Tool arguments |
| `/weather/current` | POST | Current weather | `location`: City name or coordinates |
| `/weather/forecast` | POST | Weather forecast | `location`: City name or coordinates |
| `/generate` | POST | Generate image | `prompt`: Text description<br>`negative_prompt`: Things to avoid<br>`steps`: Processing steps<br>`width`: Image width<br>`height`: Image height |
| `/speak` | POST | Text to speech | `text`: Text to convert to speech |
| `/listen` | GET | Voice input | None |

### Files and Images

| Endpoint | Method | Description | Parameters |
|----------|--------|-------------|------------|
| `/images` | GET | List available images | None |
| `/image/{path}` | GET | Serve image file | `path`: Path parameter |
| `/files/list` | POST | List files | `path`: Directory path<br>`pattern`: Filter pattern |
| `/files/move` | POST | Move file | `source`: Source path<br>`destination`: Destination path |
| `/files/copy` | POST | Copy file | `source`: Source path<br>`destination`: Destination path |
| `/files/delete` | POST | Delete file | `path`: File path |
| `/files/download` | POST | Download file | `url`: Source URL<br>`save_path`: Path to save file |

### Example API Usage

```bash
# Generate an image
curl -X POST http://localhost:8000/generate \
  -H "Content-Type: application/json" \
  -d '{"prompt": "sunset over mountains with a lake"}'

# Get weather information
curl -X POST http://localhost:8000/weather/current \
  -H "Content-Type: application/json" \
  -d '{"location": "New York"}'

# List directory contents
curl -X POST http://localhost:8000/files/list \
  -H "Content-Type: application/json" \
  -d '{"path": "C:/Users/username/Downloads"}'
```

## Tools & Capabilities

The AI can use the following tools through natural language requests:

| Tool | Description | Example Usage |
|------|-------------|---------------|
| `get_current_time` | Get the current time | "What time is it now?" |
| `get_current_date` | Get the current date | "What's today's date?" |
| `google_search` | Search the web | "Search for latest AI developments" |
| `google_image_search` | Find images | "Find me images of mountain landscapes" |
| `read_webpage` | Extract webpage content | "Summarize the content from example.com" |
| `background_check` | Get information on a person | "What can you tell me about John Smith?" |
| `listen_voice` | Convert speech to text | "Listen to my voice input" |
| `speak_text` | Convert text to speech | "Read this text aloud" |
| `analyze_pdf` | Extract PDF content | "Analyze the resume.pdf file" |
| `summarize_youtube` | Get video transcript | "Summarize the YouTube video dQw4w9WgXcQ" |
| `get_weather_current` | Current weather | "What's the weather in London?" |
| `get_weather_forecast` | Weather forecast | "Forecast for Tokyo this week" |
| `generate_image` | Create images | "Generate an image of a sunset over mountains" |
| `open_saved_image` | View saved images | "Show me saved images" |
| `list_files` | List directory contents | "Show files in Downloads folder" |
| `move_file` | Move files | "Move budget.xlsx from Downloads to Documents" |
| `copy_file` | Copy files | "Copy report.docx to my backup folder" |
| `delete_file` | Delete files | "Delete temp.txt" |
| `download_file` | Download from URL | "Download image from https://example.com/image.jpg" |

## System Architecture

### Component Diagram

```
┌─────────────────┐      ┌─────────────────┐      ┌─────────────────┐
│                 │      │                 │      │                 │
│  LM Studio or   │◄────►│   FastAPI       │◄────►│   Web Browser   │
│  AI Model Server │      │   Backend       │      │   Interface     │
│                 │      │                 │      │                 │
└─────────────────┘      └────────┬────────┘      └─────────────────┘
                                  │
                         ┌────────┴────────┐
                         │                 │
                         │   Tool Suite    │
                         │   & Utilities   │
                         │                 │
                         └─────────────────┘
```

### System Components

1. **LM Studio Backend**: Local AI model server
2. **FastAPI Server**: API endpoints and web interface
3. **Tool Functions**: Python modules implementing various capabilities
4. **Web UI**: HTML/JS interface for user interaction

### File Structure

```
/examples/
├── Scripts/
│   ├── tool_streaming_chatbot.py  # Core functionality
│   ├── chatbot_api_webaccess.py   # API server
│   └── __init__.py                # Package marker
├── static/
│   └── index.html                 # Web interface
├── images/
│   ├── generated_images/          # AI-generated images
│   └── downloaded_images/         # Google image search results
├── webui/
│   └── run.bat                    # Stable Diffusion launcher
├── docs/
│   └── chatbot_documentation.md   # This documentation
├── run_api.py                     # API startup script
└── requirements.txt               # Dependencies
```

## Installation Guide

### Prerequisites
- Python 3.8+
- [LM Studio](https://lmstudio.ai/) or compatible AI server
- [Stable Diffusion WebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) (for image generation)

### Step 1: Install Dependencies
```bash
pip install -r requirements.txt
```

Note: PyAudio may require manual installation on Windows. Download the appropriate wheel from [here](https://www.lfd.uci.edu/~gohlke/pythonlibs/#pyaudio) and install with:
```bash
pip install PyAudio-0.2.11-cp39-cp39-win_amd64.whl
```

### Step 2: Environment Configuration
Create a `.env` file with your API keys:
```
WEATHER_API_KEY=your_weather_api_key
```

### Step 3: Start LM Studio
1. Launch LM Studio
2. Load your preferred model
3. Start the local server (default: http://127.0.0.1:1234)

### Step 4: Start Stable Diffusion WebUI (Optional)
If you want image generation capabilities:
```bash
cd webui
run.bat
```

### Step 5: Launch the Assistant
```bash
python run_api.py
```

### Step 6: Access the Interface
Open http://localhost:8000 in your browser

## Security Considerations

⚠️ **Important**: This system has access to your file system. Use with caution.

- The assistant can access, modify, and delete files on your system
- Consider running in a limited user account for security
- Avoid exposing the API to the public internet without proper authentication
- Be cautious with file deletion operations

## Developer Notes

### Extending with New Tools

1. Add a new function in `tool_streaming_chatbot.py`:
```python
def my_new_tool(parameter1, parameter2):
    # Implementation here
    return {"result": "success"}
```

2. Create a tool definition:
```python
MY_NEW_TOOL = {
    "type": "function",
    "function": {
        "name": "my_new_tool",
        "description": "Description of what the tool does",
        "parameters": {
            "type": "object",
            "properties": {
                "parameter1": {"type": "string", "description": "Description of parameter"},
                "parameter2": {"type": "integer", "description": "Description of parameter"}
            },
            "required": ["parameter1"]
        }
    }
}
```

3. Add the tool to the chat_loop function's tools list:
```python
tools=[TIME_TOOL, ..., MY_NEW_TOOL]
```

4. Add a handler in the tool_calls section:
```python
elif tool_call["function"]["name"] == "my_new_tool":
    result = my_new_tool(**query_args)
```

5. Create an API endpoint in `chatbot_api_webaccess.py`:
```python
@app.post("/my_new_tool")
async def api_my_new_tool(params: MyNewToolParams):
    try:
        return my_new_tool(params.parameter1, params.parameter2)
    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))
```

### Troubleshooting

- **Model not responding**: Check that LM Studio is running and the API server is active
- **Image generation failing**: Ensure Stable Diffusion WebUI is running on port 7860
- **API endpoint errors**: Check the console logs for detailed error messages
- **File permissions**: Verify the application has appropriate access to the file system
