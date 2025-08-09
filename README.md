# Medical Triage Backend System - README

## Overview
A sophisticated AI-powered medical triage backend system that provides intelligent patient interaction through chat and voice calls, comprehensive data management, and medical report generation.

## 🚀 Core Functionalities

### 1. **AI-Powered Medical Triage Assistant (Aila)**
- **Intelligent Orchestration**: Uses OpenAI GPT-4o with tool calling capabilities
- **Clinical Question Generation**: Retrieves expert-level follow-up questions based on symptoms
- **Image Analysis**: Processes and analyzes medical images uploaded by patients
- **Safety-First Approach**: Prioritizes emergency symptom detection and 999 referrals
- **Structured Medical History Taking**: Follows clinical frameworks for comprehensive triage

### 2. **Multi-Channel Communication**
- **Text Chat Interface**: Real-time conversation with persistent history
- **Voice Call Integration**: Bland.ai integration for phone consultations
- **Image Upload Support**: Accepts and processes medical images (10MB limit)
- **Session Management**: Maintains conversation context across interactions

### 3. **Advanced Tool Integration**
- **`get_clinical_questions`**: Retrieves structured medical questions from knowledge base
- **`make_phone_call`**: Initiates voice calls when requested by patients
- **`analyze_image`**: Processes medical images for clinical assessment
- **RAG (Retrieval Augmented Generation)**: Context-aware responses using medical knowledge

### 4. **Data Persistence & Management**
- **Chat Conversation Storage**: Automatic saving to disk with JSON format
- **Call Transcript Management**: Real-time transcript capture and storage
- **User Session Handling**: Maintains state across browser sessions
- **Automatic Recovery**: Loads saved conversations after server restarts

### 5. **Comprehensive Reporting System**
- **Structured Medical Reports**: Professional format with demographics, symptoms, and clinical investigation
- **Question-Answer Checklist**: Detailed Q&A format for clinical review
- **Source Citation**: Tracks and cites medical knowledge sources
- **Downloadable Formats**: Text file generation for medical records

### 6. **REST API Endpoints**

#### Core Chat & Query
```
POST /api/query
- Handles patient messages, images, and coordinates AI responses
- Manages conversation flow and tool orchestration
- Supports multi-turn conversations with context retention
```

#### Report Generation
```
POST /api/downloadreport
- Generates comprehensive medical summary reports
- Combines chat and call data into structured clinical documents
- Provides downloadable text files with professional formatting
```

#### Data Retrieval
```
POST /api/get-history
- Returns structured JSON with separate chat and call histories
- Includes metadata and status information
- Supports both in-memory and disk-stored data
```

```
POST /api/get-raw-transcript
- Provides separated chat and call transcripts
- Returns structured JSON: {data: {call_history, chat_history}}
- No AI processing - raw conversation data only
```

#### Communication Management
```
POST /api/make-call
- Initiates voice calls through Bland.ai integration
- Includes conversation context for personalized call flow
- Automatic transcript capture and periodic updates
```

```
POST /api/refresh-conversation
- Clears conversation history for new sessions
- Resets confirmation flows and session state
- Maintains data integrity across refreshes
```

#### Confirmation & Validation
```
POST /api/initiate-confirmation
- Starts iterative summary confirmation process
- Allows patient corrections and refinements
- Maintains conversation flow until confirmed
```

### 7. **Advanced Features**

#### **Intelligent Conversation Orchestration**
- **Tool Selection**: Automatically chooses appropriate tools based on patient input
- **Context Awareness**: Maintains conversation history and clinical context
- **Multi-turn Interactions**: Handles complex medical discussions
- **Error Recovery**: Graceful handling of API failures and edge cases

#### **Medical Knowledge Integration**
- **Symptom-Based Questioning**: Retrieves relevant questions for specific symptoms
- **Clinical Frameworks**: Follows medical triage protocols
- **Source Attribution**: Tracks medical knowledge sources
- **Continuous Learning**: Updates knowledge base through interactions

#### **Call Management System**
- **Automatic Call Initiation**: Seamless transition from chat to voice
- **Transcript Synchronization**: Real-time transcript capture every 2 minutes
- **Call Status Tracking**: Monitors call progress and completion
- **Context Preservation**: Maintains conversation context across channels

#### **Data Security & Persistence**
- **File-Based Storage**: JSON format for easy backup and recovery
- **Session Management**: Secure session handling with cookies
- **CORS Configuration**: Proper cross-origin resource sharing
- **Error Logging**: Comprehensive error tracking and debugging

### 8. **Technical Specifications**

#### **Technology Stack**
- **Framework**: Node.js with Express.js
- **AI Integration**: OpenAI GPT-4o with Assistants API
- **Voice Services**: Bland.ai for phone calls
- **Session Management**: Express-session middleware
- **Data Storage**: File system JSON persistence
- **Image Processing**: Base64 data URL format

#### **Configuration Requirements**
```bash
# Environment Variables Required
OPENAI_API_KEY=your_openai_api_key
BLANDAI_API_KEY=your_blandai_api_key
```

#### **Server Configuration**
- **Port**: 4000 (configurable)
- **CORS**: Multiple origin support including localhost and production domains
- **Payload Limits**: 10MB for image uploads
- **Session Security**: HTTP-only cookies with 24-hour expiration

### 9. **File Structure & Storage**

#### **Conversation Storage**
```
conversations/
├── conversation_userId1.json
├── conversation_userId2.json
└── ...
```

#### **Transcript Storage**
```
transcripts/
├── transcript_userId1_callId1.json
├── transcript_userId1_callId2.json
└── ...
```

### 10. **Integration Capabilities**

#### **External Service Integration**
- **OpenAI Assistants**: Custom medical knowledge retrieval assistant
- **Bland.ai**: Voice call platform with custom prompts
- **n8n Workflow**: Webhook integration for workflow automation
- **ngrok**: Development tunnel support for external access

#### **Frontend Compatibility**
- **CORS-Enabled**: Supports multiple frontend origins
- **RESTful API**: Standard HTTP methods and JSON responses
- **Error Handling**: Structured error responses with appropriate status codes
- **Content-Type Support**: JSON, text, and binary data handling

## 🔧 Getting Started

### Installation
```bash
npm install
```

### Environment Setup
```bash
# Create .env file
OPENAI_API_KEY=your_openai_api_key
BLANDAI_API_KEY=your_blandai_api_key
```

### Running the Server
```bash
node backend.js
# Server starts on http://localhost:4000
```

### Development with ngrok
```bash
# Install ngrok and authenticate
ngrok authtoken your_auth_token
ngrok http 4000
# Provides public URL for external access
```

## 📋 API Response Formats

### Successful Query Response
```json
{
  "answer": "AI assistant response text"
}
```

### History Response
```json
{
  "success": true,
  "userId": "user123",
  "chatHistory": {
    "raw": [...],
    "formatted": "USER: Hello\nASSISTANT: Hi there..."
  },
  "callHistory": {
    "callId": "call123",
    "status": "completed",
    "transcript": "ASSISTANT: Hello...",
    "rawData": {...}
  },
  "combinedHistory": "Complete combined text",
  "hasCall": true,
  "callId": "call123"
}
```

### Raw Transcript Response
```json
{
  "data": {
    "call_history": "ASSISTANT: Hello, my name is Aila...",
    "chat_history": "USER: Hi\nASSISTANT: Hello! I'm Aila..."
  }
}
```

## 🛡️ Security Features
- **Input Validation**: Comprehensive request body validation
- **Error Handling**: Graceful error responses without data exposure
- **Session Security**: HTTP-only cookies with proper expiration
- **CORS Protection**: Controlled cross-origin access
- **Data Sanitization**: Proper handling of user inputs and file uploads

This backend system provides a complete medical triage solution with enterprise-grade features, comprehensive data management, and seamless integration capabilities for modern healthcare applications.
