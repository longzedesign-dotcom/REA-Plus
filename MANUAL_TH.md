# คู่มือการติดตั้งและการใช้งาน REA-Plus (ภาษาไทยแบบละเอียด)

<div align="center">

# 🛠️ REA-Plus: Reverse Engineer Anything
### ระบบวิศวกรรมย้อนกลับอัจฉริยะผ่าน Model Context Protocol (MCP)

**เข้าใจการทำงานของแอปพลิเคชันและโค้ดไบนารีระดับลึก โดยไม่ต้องมีซอร์สโค้ด**

[![Repository](https://img.shields.io/badge/GitHub-longzedesign--dotcom%2FREA--Plus-blue?style=flat-square&logo=github)](https://github.com/longzedesign-dotcom/REA-Plus)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](LICENSE)
[![Node.js](https://img.shields.io/badge/Node.js-%3E%3D18.0.0-green?style=flat-square&logo=nodedotjs)](https://nodejs.org/)

</div>

---

## 📖 สารบัญ
1. [ภาพรวมของโครงการ REA (Overview)](#1-ภาพรวมของโครงการ-rea-overview)
2. [สิ่งที่ REA สามารถวิเคราะห์ได้](#2-สิ่งที่-rea-สามารถวิเคราะห์ได้)
3. [ความต้องการของระบบ (Prerequisites)](#3-ความต้องการของระบบ-prerequisites)
4. [คู่มือการติดตั้งแบบละเอียด (Installation Guide)](#4-คู่มือการติดตั้งแบบละเอียด-installation-guide)
   - [วิธีที่ 1: ติดตั้งและตั้งค่าอัตโนมัติผ่าน npx (แนะนำ)](#วิธีที่-1-ติดตั้งและตั้งค่าอัตโนมัติผ่าน-npx-แนะนำ)
   - [วิธีที่ 2: ติดตั้งจาก Source Code ของ REA-Plus](#วิธีที่-2-ติดตั้งจาก-source-code-ของ-rea-plus)
5. [การกำหนดค่า MCP กับ AI Coding Assistants (Integration)](#5-การกำหนดค่า-mcp-กับ-ai-coding-assistants)
   - [Claude Desktop](#claude-desktop)
   - [Cursor IDE](#cursor-ide)
   - [Antigravity / Windsurf](#antigravity--windsurf)
   - [VS Code (Cline / Roo Code / Continue)](#vs-code-cline--roo-code--continue)
6. [คู่มือการใช้งาน (User Guide & Prompts)](#6-คู่มือการใช้งาน-user-guide--prompts)
   - [การสั่งการผ่าน AI Agent](#การสั่งการผ่าน-ai-agent)
   - [การใช้งานผ่าน Terminal (CLI Commands)](#การใช้งานผ่าน-terminal-cli-commands)
7. [เครื่องมือวิเคราะห์ภายนอกที่รองรับ (Engines)](#7-เครื่องมือวิเคราะห์ภายนอกที่รองรับ-engines)
8. [การแก้ไขปัญหาที่พบบ่อย (Troubleshooting & FAQ)](#8-การแก้ไขปัญหาที่พบบ่อย-troubleshooting--faq)

---

## 1. ภาพรวมของโครงการ REA (Overview)

**REA (Reverse Engineer Anything)** คือเครื่องมือวิเคราะห์และทำวิศวกรรมย้อนกลับ (Reverse Engineering) ที่สร้างขึ้นบนมาตรฐาน **Model Context Protocol (MCP)** เพื่อเชื่อมต่อความสามารถในการตรวจสอบไบนารีและแอปพลิเคชันเข้ากับ **AI Coding Agents** โดยตรง

### ประโยชน์หลัก:
- **ไม่ต้องมีซอร์สโค้ด**: สามารถแกะโครงสร้าง Logic, APIs, และฟังก์ชันการทำงานของแอปพลิเคชันที่คอมไพล์แล้วได้
- **ความแม่นยำสูง**: ส่งหลักฐาน (Evidence) เช่น Call-graphs, Disassembly, และ Decompiled Code ให้ AI วิเคราะห์อย่างเป็นระบบ
- **ความเป็นส่วนตัว**: การวิเคราะห์ทั้งหมดเกิดขึ้นบนเครื่องของคุณ (Local Execution) โค้ดและไบนารีไม่ถูกส่งออกภายนอก

---

## 2. สิ่งที่ REA สามารถวิเคราะห์ได้

| ประเภทแอปพลิเคชัน | สิ่งที่รองรับ / เทคโนโลยี | เครื่องมือที่ใช้ |
|---|---|---|
| **Native Binaries** | Mach-O (macOS), ELF (Linux), PE (Windows), C/C++, Rust, Go, Swift, Objective-C | Hopper, Ghidra, IDA Pro, Kuna |
| **Electron & Node.js** | ไฟล์แพ็กเกจ `.asar`, Source Map, Webpack/Vite Bundle, มินิฟายด์ JavaScript | AST Static Analyzer ในตัว |
| **Mobile Apps** | Android APK/AAB (DEX Bytecode, Smali, Native `.so`) | JADX, APKTool |
| **.NET Assemblies** | C#, VB.NET, F#, Unity Game Assemblies | ILSpy, Decompiler |
| **Runtime Behavior** | ดักจับการเรียกฟังก์ชัน, Memory Inspection, Dynamic Hooking | Frida |

---

## 3. ความต้องการของระบบ (Prerequisites)

- **Node.js**: เวอร์ชั่น `18.17.0` หรือ `20.x` ขึ้นไป
- **npm** หรือ **pnpm**
- **ระบบปฏิบัติการที่รองรับ**:
  - macOS (Apple Silicon M1/M2/M3/M4 หรือ Intel)
  - Linux (Ubuntu 20.04+, Debian, Fedora, Arch)
  - Windows 10/11 (แนะนำรันผ่าน WSL2 หรือ PowerShell)
- *(ทางเลือก)*: ติดตั้งโปรแกรมวิเคราะห์เนทีฟ เช่น **Hopper Disassembler**, **Ghidra** (ต้องมี Java 17+), หรือ **Frida** หากต้องการวิเคราะห์ไบนารีเนทีฟระดับลึก

---

## 4. คู่มือการติดตั้งแบบละเอียด (Installation Guide)

### วิธีที่ 1: ติดตั้งและตั้งค่าอัตโนมัติผ่าน npx (แนะนำ)

วิธีนี้เป็นวิธีที่ง่ายที่สุด ระบบจะทำการตรวจหา AI Agent ที่ติดตั้งอยู่ในเครื่อง และตั้งค่าเชื่อมโยง MCP ให้อัตโนมัติ:

```bash
npx rea-agents setup
```

**ขั้นตอนการทำงาน:**
1. ระบบจะแสดงรายการ AI Agent ที่ตรวจพบในเครื่อง (เช่น Claude Desktop, Cursor, Gemini CLI เป็นต้น)
2. เลือก Agent ที่คุณใช้งาน
3. ระบบจะบันทึกการตั้งค่า MCP ลงใน Config ของ Agent นั้นๆ พร้อมสำรองไฟล์เดิมไว้ให้
4. รีสตาร์ท AI Agent เพื่อเริ่มต้นใช้งาน

---

### วิธีที่ 2: ติดตั้งจาก Source Code ของ REA-Plus

หากคุณต้องการพัฒนาต่อยอด แก้ไขโค้ด หรือรันบนเซิร์ฟเวอร์เฉพาะ:

```bash
# 1. Clone repository
git clone https://github.com/longzedesign-dotcom/REA-Plus.git
cd REA-Plus

# 2. ติดตั้ง Dependencies
npm install

# 3. คอมไพล์โปรเจกต์ TypeScript
npm run build

# 4. ทดสอบรันการทำงาน
node ./dist/cli/index.js --help
```

---

## 5. การกำหนดค่า MCP กับ AI Coding Assistants

หากต้องการเพิ่ม REA Server เข้าไปใน AI Agent ด้วยตนเอง สามารถใช้การตั้งค่าด้านล่างนี้:

### Claude Desktop
แก้ไขไฟล์การตั้งค่า:
- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "rea": {
      "command": "npx",
      "args": ["-y", "rea-agents@latest", "mcp-server"]
    }
  }
}
```

### Cursor IDE
ไปที่ **Cursor Settings** > **Features** > **MCP Servers** > **Add New MCP Server**:
- **Name**: `rea`
- **Type**: `command`
- **Command**: `npx -y rea-agents@latest mcp-server`

หรือใส่ใน `.cursor/mcp.json` ของโปรเจกต์:
```json
{
  "mcpServers": {
    "rea": {
      "command": "npx",
      "args": ["-y", "rea-agents@latest", "mcp-server"]
    }
  }
}
```

### Antigravity / Windsurf
แก้ไขไฟล์ `~/.codeium/windsurf/mcp_config.json`:
```json
{
  "mcpServers": {
    "rea": {
      "command": "npx",
      "args": ["-y", "rea-agents@latest", "mcp-server"]
    }
  }
}
```

### VS Code (Cline / Roo Code / Continue)
ใส่ในส่วนการตั้งค่า MCP Server:
```json
{
  "name": "rea",
  "command": "npx",
  "args": ["-y", "rea-agents@latest", "mcp-server"]
}
```

---

## 6. คู่มือการใช้งาน (User Guide & Prompts)

### การสั่งการผ่าน AI Agent
เมื่อเชื่อมต่อ MCP เรียบร้อยแล้ว คุณสามารถพิมพ์คุยกับ AI เป็นภาษาไทยหรืออังกฤษได้ทันที:

#### ตัวอย่าง 1: วิเคราะห์แอปพลิเคชัน Electron / โปรแกรมที่ชอบ
> *"ช่วยตรวจสอบไฟล์ `/Applications/Slack.app/Contents/Resources/app.asar` ให้หน่อย ว่าระบบจัดเก็บข้อความ Local Cache ทำงานอย่างไร และใช้โครงสร้างตารางแบบไหน"*

#### ตัวอย่าง 2: ตรวจสอบ Native Binary หาจุดเรียก API และ Encryption
> *"วิเคราะห์ไบนารี `/usr/local/bin/target-binary` ดูว่ามีฟังก์ชันไหนทำการเข้ารหัสข้อมูลก่อนส่งออกเครือข่ายบ้าง พร้อมแสดง Decompiled Code และพารามิเตอร์"*

#### ตัวอย่าง 3: ดักจับข้อมูล Runtime ด้วย Frida
> *"ดักจับฟังก์ชัน `auth_verify_token` ของโปรเซสเป้าหมาย แสดง Argument และ Return value ที่ส่งออกมา"*

---

### การใช้งานผ่าน Terminal (CLI Commands)

คุณสามารถสั่งการ REA ผ่าน Terminal โดยตรงได้เช่นกัน:

#### 1. วิเคราะห์แอป JavaScript / Electron / ASAR
```bash
npx -y rea-agents@latest analyze-javascript-application /path/to/app.asar --json
```

#### 2. ตรวจสอบข้อมูลเชิงลึกของ Native Binary
```bash
npx -y rea-agents@latest analyze-native-binary /path/to/binary --json
```

#### 3. ค้นหาสตริงและ API Endpoint ภายในไบนารี
```bash
npx -y rea-agents@latest extract-strings /path/to/binary --filter "https://"
```

---

## 7. เครื่องมือวิเคราะห์ภายนอกที่รองรับ (Engines)

REA ออกแบบมาให้ทำงานร่วมกับซอฟต์แวร์วิศวกรรมย้อนกลับมาตรฐานสากล:

1. **Hopper Disassembler** *(macOS & Linux)*:
   - เหมาะที่สุดสำหรับ Mach-O และ ELF, ทำงานรวดเร็วมาก สามารถ Decompile โค้ด Objective-C / Swift / C ได้อย่างสวยงาม
2. **Ghidra** *(Cross-platform)*:
   - ฟรี Open Source พัฒนาโดย NSA รองรับเกือบทุกสถาปัตยกรรม (x86, ARM, MIPS, RISC-V)
3. **IDA Pro**:
   - ซอฟต์แวร์ระดับอุตสาหกรรม REA รองรับการเชื่อมต่อผ่าน IDA Python Bridge
4. **Frida Dynamic Toolkit**:
   - ดักจับการทำงานแบบ Runtime โดยไม่ต้องแก้ไขไฟล์ต้นฉบับ

---

## 8. การแก้ไขปัญหาที่พบบ่อย (Troubleshooting & FAQ)

### Q1: ขึ้นแจ้งเตือน Linux owned-process prerequisite
**วิธีแก้**: บนระบบปฏิบัติการ Linux ต้องติดตั้งแพ็กเกจ `procps` เพื่อให้ REA ตรวจสอบสิทธิ์และโปรเซสได้:
```bash
sudo apt-get install procps  # Ubuntu/Debian
sudo dnf install procps-ng   # Fedora/CentOS
```

### Q2: Ghidra หา Java ไม่พบ
**วิธีแก้**: ตรวจสอบว่าได้ติดตั้ง JDK 17 หรือ 21 แล้ว และตั้งค่า Path:
```bash
export JAVA_HOME=/path/to/jdk-17
export PATH=$JAVA_HOME/bin:$PATH
```

### Q3: MCP Server ไม่ตอบสนอง
**วิธีแก้**: ลองทดสอบรันคำสั่งโดยตรงบน Terminal เพื่อตรวจสอบ Error:
```bash
npx rea-agents mcp-server
```

---

## 📄 ลิขสิทธิ์และการใช้งาน (License)
ต้นฉบับพัฒนาโดย [morluto/rea](https://github.com/morluto/rea) ภายใต้ลิขสิทธิ์ **MIT License**  
นำมาดูแลและพัฒนาต่อยอดสำหรับ REA-Plus โดย [longzedesign-dotcom](https://github.com/longzedesign-dotcom/REA-Plus)
