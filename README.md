# Lexi - AI Voice Assistant for Department of Computer Science

Lexi is an intelligent voice assistant built on ESP32-S3 that serves as a smart receptionist for the Department of Computer Science. It can interact with visitors, provide assistance, and maintain conversation history while the department head is away.

## 🎯 Features

- **Voice Interaction**: Real-time speech-to-text and text-to-speech capabilities
- **Proximity Detection**: Automatically detects when someone approaches using ultrasonic sensor
- **Conversation Memory**: Maintains context throughout conversations and provides summaries
- **HOD Detection**: Recognizes when the department head returns and provides interaction reports
- **LED Status Indicators**: Visual feedback for different system states
- **Audio Streaming**: High-quality audio playback with MP3 decoding

## 🔧 Hardware Requirements

### ESP32-S3 Board
- **Board**: Freenove ESP32-S3 WROOM
- **Features**: PSRAM enabled for audio buffering
- **Framework**: Arduino

### Components
- **Microphone**: I2S digital microphone (INMP441 or similar)
- **Speaker**: I2S audio output (MAX98357A or similar)
- **Ultrasonic Sensor**: HC-SR04 for proximity detection
- **LEDs**: RGB status indicators
- **Optional**: Servo motor for physical interactions

### Pin Configuration
```
I2S Audio:
- BCLK: GPIO 42
- WS (LRCLK): GPIO 41
- DIN (Mic): GPIO 7
- DOUT (Speaker): GPIO 6

Sensors:
- Ultrasonic TRIG: GPIO 10
- Ultrasonic ECHO: GPIO 11
- Servo: GPIO 9
- Mic Power: GPIO 8

Status LEDs:
- Red LED: GPIO 15
- Green LED: GPIO 16
- Blue LED: GPIO 17
```

## 📋 Dependencies

### PlatformIO Libraries
- [arduino-audio-tools](https://github.com/pschatzmann/arduino-audio-tools.git)
- [arduino-libhelix](https://github.com/pschatzmann/arduino-libhelix.git)
- ESP32Servo
- HCSR04
- ArduinoJson

### API Services
- **OpenAI API**: Whisper for STT, GPT-3.5-turbo for conversations
- **ElevenLabs API**: Text-to-speech voice synthesis

## 🚀 Setup Instructions

### 1. Hardware Assembly
1. Connect I2S microphone and speaker to specified GPIO pins
2. Wire HC-SR04 ultrasonic sensor for proximity detection
3. Connect RGB LEDs for status indication
4. Ensure proper power supply (5V recommended)

### 2. Software Configuration
1. Clone this repository
2. Create `config.h` file with your credentials:
```cpp
#define WIFI_SSID "your_wifi_ssid"
#define WIFI_PASS "your_wifi_password"
#define OPENAI_API_KEY "your_openai_api_key"
#define ELEVENLABS_API_KEY "your_elevenlabs_api_key"
```

### 3. Build and Upload
1. Open project in PlatformIO
2. Build and upload to ESP32-S3
3. Monitor serial output for debugging

## 🎭 How It Works

### State Machine Flow
1. **Greeting**: Initial welcome message when system starts
2. **Wait**: Monitors for person within 1 meter using ultrasonic sensor
3. **Countdown**: 1-second preparation before recording
4. **Record**: Captures 5 seconds of audio
5. **Process**: Transcribes speech, generates AI response, and plays TTS
6. **Next**: Brief pause before returning to wait state

### LED Status Indicators
- **Red**: System powered on/processing
- **Blue**: WiFi connected/waiting for interaction
- **Green**: Recording audio input

### Special Commands
- **HOD Return**: "Lexi it's me I am available now" triggers summary report generation

## 🧠 AI Personality

Lexi is programmed with a specific personality:
- Professional and polite tone
- Remembers conversation context
- Assists visitors when department head is unavailable
- Provides detailed interaction summaries to department head
- Maintains conversational memory across interactions

## 📁 Project Structure

```
├── src/
│   └── main.cpp          # Main application code
├── include/
│   └── README            # Header files documentation
├── lib/
│   └── README            # Private libraries
├── test/
│   └── README            # Test version of main code
├── .vscode/
│   ├── extensions.json   # VS Code extensions
│   └── settings.json     # VS Code settings
├── platformio.ini        # PlatformIO configuration
├── .gitignore           # Git ignore rules
└── config.h             # WiFi and API credentials (create this)
```

## ⚙️ Configuration Options

### Audio Settings
- Sample Rate: 16kHz
- Bit Depth: 16-bit
- Recording Duration: 5 seconds
- Speaker: Stereo output
- Microphone: Mono input

### AI Settings
- STT Model: OpenAI Whisper-1
- Chat Model: GPT-3.5-turbo
- TTS Voice: ElevenLabs (configurable voice ID)
- Conversation History: Limited to 10 messages

### Detection Settings
- Proximity Range: 1 meter (100cm)
- Countdown Timer: 1 second
- Processing Timeout: Configurable per API

## 🔧 Customization

### Changing AI Personality
Modify the personality strings in `main.cpp`:
```cpp
String p1 = "You are Lexi, the smart AI assistant...";
// Add your custom personality traits
```

### Adjusting Audio Quality
Modify I2S configuration in `setup()` and audio processing functions.

### Hardware Pin Changes
Update pin definitions at the top of `main.cpp` to match your hardware setup.

## 🐛 Troubleshooting

### Common Issues
1. **WiFi Connection**: Verify SSID and password in config.h
2. **API Errors**: Check API keys and internet connectivity
3. **Audio Issues**: Verify I2S wiring and power supply
4. **Memory Errors**: Ensure PSRAM is enabled in platformio.ini

### Debug Output
Monitor serial output at 115200 baud for detailed logging and error messages.



