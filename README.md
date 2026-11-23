# UI Testing Lab (ITDS362)

คลังเก็บโค้ดการทดสอบ UI อัตโนมัติสำหรับรายวิชา ITDS362 Software Quality Assurance and Testing โดยทำการทดสอบบนเว็บไซต์ตัวอย่าง 2 เว็บไซต์ ได้แก่ Selenium Web Form และ QuickPizza

## เครื่องมือที่เลือกใช้ในการทดสอบ (Chosen Tools)
การทดลองนี้เลือกทำ 2 เครื่องมือ ได้แก่:
1. **Part 3: Robot Framework** (โฟลเดอร์ `robot/`)
2. **Part 4: Cypress** (โฟลเดอร์ `cypress/`)

---

## โครงสร้างและการรันการทดสอบ

### 1. Robot Framework (`robot/`)
โฟลเดอร์นี้บรรจุชุดการทดสอบ UI อัตโนมัติที่พัฒนาด้วย Robot Framework และ SeleniumLibrary ร่วมกับ ChromeDriver เพื่อควบคุมการทำงานของ Google Chrome โดยออกแบบตามแนวคิด Keyword-Driven และ Page Object Pattern ผ่าน Resource file

- **โครงสร้างไฟล์สำคัญ**:
  - `requirements.txt`: ระบุเวอร์ชันของไลบรารี (`robotframework==7.5.1`, `robotframework-seleniumlibrary==6.9.0`)
  - `first.robot`: การทดสอบเริ่มต้นบน Selenium Web Form และการตรวจสอบชื่อเรื่องหน้าเว็บ
  - `web_form.robot`: การทดสอบการกรอกและส่งฟอร์มครบทุกชนิดข้อมูล (Text, Password, Textarea, Select, Checkbox, Radio, Date)
  - `resources/quickpizza.resource`: Resource file รวมคีย์เวิร์ดระดับหน้าจอและ Locator สำหรับ QuickPizza
  - `pizza_scenario.robot`: การทดสอบกระบวนการ QP-01 ครบทั้ง 7 ขั้นตอน (Customer flow จนถึง Admin flow)
  - `results/run1/` และ `results/run2/`: หลักฐานผลการรันแบบ Headless 2 รอบเต็ม ประกอบด้วย `report.html`, `log.html`, `output.xml` และภาพถ่ายหน้าจอ

- **คำสั่ง 1 บรรทัดสำหรับรันการทดสอบ**:
  ```bash
  robot -d results/run1 -v BROWSER:headlesschrome pizza_scenario.robot
  ```
  *(หมายเหตุ: ต้องเปิดใช้งาน Virtual Environment ใน `robot/.venv` และกำหนดค่า Environment Variable `CHROMEDRIVER` ก่อนรัน)*

---

### 2. Cypress (`cypress/`)
โฟลเดอร์นี้บรรจุชุดการทดสอบ End-to-End (E2E) UI Testing ที่พัฒนาด้วย Cypress (JavaScript) โดยทำงานและทดสอบภายในเบราว์เซอร์โดยตรง พร้อมทั้งดักจับและตรวจสอบ Network request ผ่าน `cy.intercept` และบันทึกหลักฐานวิดีโอรวมถึงภาพถ่ายหน้าจออัตโนมัติ

- **โครงสร้างไฟล์สำคัญ**:
  - `package.json`: กำหนดค่าโปรเจกต์และ Dependency (`cypress@16.2.1`)
  - `cypress.config.js`: ตั้งค่า Viewport (1280x1600), Default Command Timeout (10000ms) และเปิดการบันทึกวิดีโอ (`video: true`)
  - `cypress/support/commands.js`: Custom Command `cy.adminLogin(user, password)`
  - `cypress/e2e/web-form.cy.js`: การทดสอบ Selenium Web Form
  - `cypress/e2e/pizza-scenario.cy.js`: การทดสอบกรณีทดสอบ QP-01 ขั้นตอนที่ 1–7 พร้อมการตรวจสอบความสอดคล้องระหว่าง API Payload กับการแสดงผลหน้าเว็บ
  - `cypress/videos/`: วิดีโอบันทึกการทดสอบ (`web-form.cy.js.mp4`, `pizza-scenario.cy.js.mp4`)
  - `cypress/screenshots/`: ภาพถ่ายหน้าจอหลักฐานขั้นตอนที่ 4, 5 และ 7

- **คำสั่ง 1 บรรทัดสำหรับรันการทดสอบ**:
  ```bash
  npx cypress run --browser chrome
  ```
  *(หรือระบุ spec เช่น `npx cypress run --browser chrome --spec "cypress/e2e/pizza-scenario.cy.js"`)*
