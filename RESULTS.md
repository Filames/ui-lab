# UI lab results
Tool parts that I chose: Part 3 (Robot Framework) and Part 4 (Cypress)

## Part 0
- Step 0.1, Chrome version: Google Chrome 158.0.8127.36 (Official Build) (64-bit)
- Step 0.3, ChromeDriver version: ChromeDriver 158.0.8127.36 (fba7493a3885d519b5bf67c3e1e92d7fc3db4209-refs/branch-heads/8127@{#628})
- Step 0.4, locator of Pizza, Please!: `name:pizza-please` (or `button[name='pizza-please']`)
- Step 0.4, locator of Excluded tools: `css:select` (or `select`)
- Step 0.4, locator of Must be vegetarian: `id:default-checkbox` (or `#default-checkbox`)
- Step 0.4, locator of Custom Pizza Name: `css:input#pizza-name` (or `input#pizza-name`)
- Step 0.5, pizza name, tool and calories of the run by hand:
  - Pizza name: Lab Pizza 20261123-142014
  - Tool: Scissors
  - Calories per slice: 550

| Step | Actual result | Pass or fail |
|------|---------------|--------------|
| 1 | The heading reads "Looking to break out of your pizza routine?" | Pass |
| 2 | Advanced fields appeared: Max Calories per Slice, Excluded tools, Must be vegetarian, and Custom Pizza Name | Pass |
| 3 | Max Calories per Slice set to 600, Knife selected in Excluded tools, Must be vegetarian checked, Custom Pizza Name set to "Lab Pizza 20261123-142014" | Pass |
| 4 | Pizza recommended with Name: "Lab Pizza 20261123-142014", Tool: "Scissors" (not Knife), and Calories per slice: "550" (<= 600) | Pass |
| 5 | Clicked "Love it!" without logging in, message displayed: "Please log in first." | Pass |
| 6 | Navigated to /admin, signed in with admin/admin, page displayed "Latest pizza recommendations" | Pass |
| 7 | Located "Lab Pizza 20261123-142014" in list with tool "Scissors" matching step 4 | Pass |

## Part 3: Robot Framework
- robot --version: Robot Framework 7.5.1 (Python 3.13.1 on win32)
- Step 3.2, failure message for the wrong title: Title should have been 'Web Form' but was 'Web form'.
- Step 3.8, run 1, result, elapsed time and pizza name:
  - Result: PASS
  - Elapsed time: 15.08s (Total suite: 15.50s)
  - Pizza name: Lab Pizza 20261123-142114
- Step 3.8, run 2, result, elapsed time and pizza name:
  - Result: PASS
  - Elapsed time: 10.96s (Total suite: 11.38s)
  - Pizza name: Lab Pizza 20261123-142142

## Part 4: Cypress
- node --version, npm --version and npx cypress version:
  - node --version: v24.14.0
  - npm --version: 11.13.0
  - npx cypress version: 16.2.1
- Step 4.3, failure message for the wrong title: Timed out retrying after 10000ms: expected 'Web form' to equal 'Web Form'
- Step 4.6, tool and calories in the answer of POST /api/pizza:
  - Tool: Scissors
  - Calories: 350
- Step 4.7, the line of the admin list that shows your pizza: Lab Pizza 20261123-151608 (tool=Scissors, ingredients_number=5) (newest)
- Step 4.8, run 1, result, duration and pizza name:
  - Result: All specs passed! (2 passing)
  - Duration: web-form.cy.js 00:04, pizza-scenario.cy.js 00:14 (Total: 00:18)
  - Pizza name: Lab Pizza 20261123-151227
- Step 4.8, run 2, result, duration and pizza name:
  - Result: All specs passed! (2 passing)
  - Duration: web-form.cy.js 00:05, pizza-scenario.cy.js 00:13 (Total: 00:18)
  - Pizza name: Lab Pizza 20261123-151323
- Step 4.9 (optional), browser and result:
  - Browser: Microsoft Edge 158 (headless)
  - Result: All specs passed! (pizza-scenario.cy.js passed in 15s, web-form.cy.js passed in 4s)

## Part B
1. การระบุ element ของกล่อง "Max Calories per Slice" ผ่าน label แทน id เป็นเพราะว่าในหน้า QuickPizza กล่องตัวเลขทั้งสามช่อง (Max Calories, Max Toppings, Max Price) มีการใช้ id ซ้ำกันคือ `floating_filled` ซึ่งขัดกับมาตรฐาน HTML ที่ id ต้องไม่ซ้ำกัน ส่งผลให้ locator `#floating_filled` ชี้ไปยัง element ถึง 3 ตัวและไม่สามารถระบุช่องที่ต้องการได้อย่างเจาะจง ข้อเสนอแนะในการปรับปรุงคือผู้พัฒนาควรเปลี่ยน id ของแต่ละช่องให้สื่อความหมายเฉพาะตัวและไม่ซ้ำกัน เช่น `id="max-calories"`, `id="max-toppings"` เป็นต้น การปรับปรุงนี้จะช่วยให้ผู้ทดสอบสามารถใช้ locator ระดับ id ได้โดยตรง ซึ่งสั้น กระชับ ทำงานรวดเร็ว และทนทานต่อการเปลี่ยนแปลงโครงสร้างหรือข้อความในหน้าเว็บ

2. การเปรียบเทียบค่าเครื่องมือ (tool) ในรายการ admin กับผลลัพธ์ใน recommendation จัดเป็น **Consistency Test Oracle** (หรือ Self-consistency check) ที่ตรวจสอบความสอดคล้องของข้อมูลระหว่างสองส่วนของระบบที่ทำงานประสานกัน การตรวจสอบนี้สามารถตรวจจับข้อผิดพลาด (fault) ในกรณีที่ QuickPizza ตอบกลับผลลัพธ์พิซซ่าแก่ลูกค้าอย่างหนึ่ง แต่ตัว backend service กลับส่งข้อมูลไม่สมบูรณ์ บันทึกลงฐานข้อมูลผิดพลาด หรือฟิลด์เครื่องมือตกหล่นระหว่างบันทึก ทำให้ฝั่งหน้าจอผู้ดูแลระบบแสดงผลเครื่องมือไม่ตรงกับที่ลูกค้าได้รับ

3. การตรวจสอบว่าพิซซ่าที่ได้เป็นมังสวิรัติ (Vegetarian) จริง สามารถทำได้โดยตรวจยืนยันว่าส่วนผสม (ingredients) ทั้งหมดที่ถูกเลือกมานั้นไม่มีเนื้อสัตว์เลย โดยการทดสอบต้องการข้อมูลรายการส่วนผสมของพิซซ่า ซึ่งสามารถดึงได้จาก payload การตอบกลับของคำขอ `POST /api/pizza` ผ่านอ็อบเจกต์ `response.body.pizza.ingredients` หรือฟิลด์ `response.body.vegetarian === true` รวมทั้งอาจใช้รายการอ้างอิงของวัตถุดิบมังสวิรัติจาก API สารบบวัตถุดิบเพื่อตรวจสอบว่าไม่มีส่วนผสมต้องห้ามปนเปื้อน

4. ทั้งสองเครื่องมือมีกลไกและการใช้งานที่แตกต่างกันอย่างชัดเจน โดย Cypress มีระบบ built-in automatic retry ในตัว ทำให้คำสั่งเช็คและค้นหาองค์ประกอบรอการปรากฏของ element และข้อมูลของ QuickPizza ได้อย่างราบรื่นโดยไม่ต้องเขียนคำสั่ง wait แบบ manual ในขณะที่ Robot Framework ต้องใช้ keyword จำพวก `Wait Until Page Contains Element` หรือกำหนด explicit timeout ให้กับ SeleniumLibrary ในด้านความเร็วในการพัฒนา Cypress เขียนด้วย JavaScript ทำให้สามารถดักจับและตรวจสอบ Network request (`cy.intercept`) ได้สะดวกรวดเร็วมาก ส่วน Robot Framework มีจุดเด่นด้านความอ่านง่ายของ Keyword ภาษาธรรมชาติ และเมื่อเปรียบเทียบรายงานผล Robot Framework มีไฟล์ `log.html` และ `report.html` ที่สรุปผลขั้นตอนได้อย่างเป็นลำดับและดูง่ายมาก ในขณะที่ Cypress มีไฟล์วิดีโอ `.mp4` ที่บันทึกการทำงานของหน้าจอพร้อม Command log ด้านข้าง ทำให้เห็นข้อผิดพลาดได้ชัดเจนแบบเรียลไทม์

5. เครื่องมือที่เลือกใช้สำหรับการทดสอบ UI อัตโนมัติใน Project 2 คือ **Cypress** โดยมีเหตุผลสนับสนุนสองประการจากผลการทำแล็บนี้ ประการแรกคือ Cypress มีกลไก Smart Automatic Waiting และมีคำสั่ง `cy.intercept()` ที่ช่วยให้สามารถตรวจสอบทั้งส่วนติดต่อผู้ใช้ (UI DOM) และข้อมูลที่รับส่งทาง Network API ควบคู่กันได้ในการทดสอบเดียว ทำให้ลดปัญหา flaky test และตรวจจับข้อผิดพลาดระดับ integration ได้แม่นยำ ประการที่สองคือ Cypress สามารถบันทึกวิดีโอ (.mp4) และภาพถ่ายหน้าจอของการทดสอบให้อัตโนมัติในระหว่างรันแบบ headless ทำให้มีหลักฐานการทดสอบที่สมบูรณ์และสะดวกต่อการตรวจสอบย้อนหลังของทีมงาน

6. สำหรับ Robot Framework หาก QuickPizza มีการเปลี่ยนแปลง locator ของกล่องข้อความ calories เราจะต้องแก้ไขเพียงจุดเดียวที่ส่วนของ `*** Variables ***` ในไฟล์ resource คือตัวแปร `${MAX CALORIES}` ใน `robot/resources/quickpizza.resource` โดยไม่ต้องแตะต้องหรือแก้ไขโค้ดใดๆ ในไฟล์ทดสอบ `pizza_scenario.robot` เลย เนื่องจากไฟล์ทดสอบเรียกใช้ระดับ Business Keyword คือ `Fill In The Pizza Request` ซึ่งโครงสร้าง Resource file ทำหน้าที่เป็น abstraction layer (Page Object Pattern) กั้นรายละเอียดทางเทคนิคของ locator ให้อยู่เฉพาะใน resource file ทำให้การบำรุงรักษาชุดทดสอบทำได้สะดวกรวดเร็วเมื่อหน้าเว็บเปลี่ยน

7. ใน Cypress การทดสอบ QP-01 จำเป็นต้องรวมเป็น Single test (ภายใน `it()` เดียวกัน) เนื่องจาก Cypress มีพฤติกรรมมาตรฐานด้าน Test Isolation ที่จะล้าง cookies, session และ local storage ทั้งหมดก่อนเริ่มแต่ละ test case เสมอ และเนื่องจากระบบ QuickPizza กระจายการทำงานอยู่บนหลายเซิร์ฟเวอร์โดยมีคุกกี้สติกกี้ (เช่น AWSALB / Session Cookie) ยึดโยงเบราว์เซอร์ไว้กับเซิร์ฟเวอร์เครื่องเดิม หากแยกขั้นตอนที่ 6 และ 7 (Admin part) ออกเป็น test ที่สอง คุกกี้จะถูกล้าง ทำให้เบราว์เซอร์อาจถูกส่งต่อไปยังเซิร์ฟเวอร์คนละตัวกับที่ลูกค้าสร้างพิซซ่าไว้ในขั้นตอนที่ 4 ส่งผลให้การค้นหาชื่อพิซซ่าในรายการ admin ล้มเหลวและไม่พบข้อมูล
