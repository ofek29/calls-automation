# Mortgage Call Automation System for NIVISION

## Overview

An intelligent, automated system for processing Hebrew mortgage advice call recordings. Built on MAKE platform, this solution transcribes calls, extracts structured data, and delivers actionable insights to call center managers.

### Key Features

-  **Hebrew Language Support** - Native Hebrew transcription and analysis
-  **ASR System** - OpenAI Whisper with Google Speech-to-Text fallback
-  **AI-Powered Analysis** - GPT-4 extracts client info, mortgage needs, and sentiment
-  **Quality Assurance** - Automatic flagging of low-confidence calls for human review
-  **Confidence Scoring** - Every extracted field includes a confidence score (0.0-1.0)
-  **Error Handling** - Comprehensive retry logic and fallback mechanisms

### What It Does

1. **Receives** audio files from your call center system
2. **Transcribes** Hebrew speech with speaker identification (agent vs. client)
3. **Analyzes** conversation to extract:
   - Client information (name, phone, email, preferred contact time)
   - Mortgage needs (type, amount, property, timeline)
   - Objections and concerns
   - Required follow-up actions
   - Compliance flags
   - Sentiment analysis
4. **Validates** data quality and flags low-confidence calls for review
5. **Notifies** managers via email with Hebrew summaries

## Quality Assurance System

### Fallback Route Approach

The system implements a **dual-ASR strategy** to ensure reliable transcription even with challenging audio quality:

**Primary Route: OpenAI Whisper**
- First attempt uses OpenAI Whisper API (excellent Hebrew support)
- Calculates overall confidence score from segment-level scores
- Threshold: confidence ≥ 0.75 proceeds to analysis

**Fallback Route: Google Speech-to-Text**
- Automatically triggered when Whisper confidence < 0.65
- Uses Google Cloud Speech-to-Text API with Hebrew language pack
- Provides alternative transcription with different acoustic models
- System selects the higher-confidence transcription

**Decision Flow:**
```
Audio → Whisper Transcription → Confidence Check
                                      ↓
                        ┌─────────────┴──────────────┐
                        ↓                            ↓
                 Confidence ≥ 0.75          Confidence < 0.75
                        ↓                            ↓
                  LLM Analysis              Google Fallback ASR
                                                     ↓
                                            Compare Confidences
                                                     ↓
                                            Use Best Result
```


### Documentation

##  Quick Start

### Prerequisites

-  MAKE account (sign up at https://www.make.com)
-  OpenAI API key with credits
-  Google cloud API key
-  Email account 


### Deployment Steps

1. **Import Blueprint**

   ```
   MAKE → Scenarios → Import Blueprint → Upload make-blueprint.json
   ```

2. **Configure API's**

   - Add API keys (OpenAI, Google, Dropbox)
   - Configure email addresses


4. **Test**

   - Send test audio file to storage
   - Verify output
   - Check email notifications

5. **Activate**
   - Toggle scenario to ON
   - Integrate with your call center system



##  System Architecture

```
Call Center →  Audio Storage → Transcription (Whisper/Google)
→ LLM Analysis (GPT-4) → Validation → Storage → Notifications (Email)
```

