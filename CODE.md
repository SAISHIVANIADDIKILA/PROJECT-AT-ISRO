
## 📌 Aim

To control 3-phase induction motors (Star-Delta contactors) using a PLC-SCADA system.

---

## ⚙️ Technologies Used

| Component                | Details                          |
|--------------------------|----------------------------------|
| **PLC**                  | Siemens S7-315 2 PN/DP           |
| **PLC Coding Software**  | Step7 5.5                        |
| **Coding Language**      | Ladder Logic                     |
| **SCADA System**         | WinCC 7.4                        |
| **Communication**        | Profibus, Ethernet TCP/IP        |
| **Operating System**     | Windows 10 Pro                   |

---

## 🔧 PLC Technical Specifications

- Model No: `6ES7 315-2EH14-0AB0`
- 384 KB Work Memory
- 0.05ms/1000 instructions
- PROFINET (RT/IRT), MPI/DP communication
- Up to 32 module configuration
- Firmware: V3.2

---

## 🔌 PLC Hardware Configuration

- **Slot 1**: Power supply module for CPU  
- **Slot 2**: CPU module (Profibus address: 2)  
- Communication with WinCC SCADA via Profibus

---

## 🧠 PLC Code Development

### Step 1: Digital Inputs (DI)
| Function      | Address |
|---------------|---------|
| Motor Start   | M10.0   |
| Motor Stop    | M10.1   |

### Step 2: Digital Outputs (DO)
| Function              | Address |
|-----------------------|---------|
| Main Contactor ON     | M10.2   |
| Star Contactor ON     | M10.5   |
| Delta Contactor ON    | M10.6   |

### Step 3: Data Variable
- **Run Timer**: `DB2.DBD22` (32-bit Real)

---

## 🧱 Blocks Used

- **Data Block (DB2)**
- **Function Block (FB1, FB2)**
- **Function Call (FC)**

---

## 🖥️ SCADA Development (WinCC)

### Steps:
1. Tag Creation
2. Main Screen Design
3. Motor Control Mimic & Popup
4. Emergency Button Popup
5. Exit Button Scripting (C-Script)

### Main Screen Features:
- Motor control menu
- Soft emergency button
- Alarm view
- Exit SCADA

### Motor Control Sequence:
1. Press Start → Main & Star contactors ON
2. After 6s → Star OFF, Delta ON
3. Timer runs during operation
4. Press Stop → Motor stops, timer resets

### Emergency Button:
- Popup: “Do you want to activate emergency?”
- On YES → Motor stops
- On NO → No action

### Exit Button Script (WinCC C):

```c
#include "apdefap.h"

void OnLButtonDown(char* lpszPictureName, char* lpszObjectName, char* lpszPropertyName, UINT nFlags, int x, int y)
{ 
    int p=0;
    p = MessageBox(NULL, "Do You Really Want to Exit SCADA", "SCADA Exit Confirmation", MB_YESNO|MB_SYSTEMMODAL);
    if (p == 6) {
        ExitWinCC();
    }
}
