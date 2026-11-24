# ui-lab

Lab Automated UI Testing ของวิชา ITDS362 ส่วนของเครื่องมือที่เลือกทำคือ **Part 3 (Robot Framework)** และ **Part 4 (Cypress)** ทั้งสองส่วนทำ test case QP-01 บน QuickPizza แบบอัตโนมัติ และทดสอบ web form ของ Selenium ผลการทดลองและคำตอบของ Part B อยู่ใน [RESULTS.md](RESULTS.md)

## robot/ (Part 3, Robot Framework)

ใช้ Robot Framework 7.5.1 กับ SeleniumLibrary 6.9.0 ไฟล์ `first.robot` และ `web_form.robot` ทดสอบ web form ส่วน `pizza_scenario.robot` รัน QP-01 โดยใช้ keyword จาก `resources/quickpizza.resource` โฟลเดอร์ `results/run1` และ `results/run2` เก็บ log, report และ screenshot ของการรันเต็มสองรอบ

ต้องมี ChromeDriver ที่ตรงกับเวอร์ชันของ Chrome และตั้ง path ไว้ในตัวแปร environment `CHROMEDRIVER` (ดู Step 0.3) รันเทสต์จากโฟลเดอร์ `robot`:

```bash
cd robot
python3 -m venv .venv && source .venv/bin/activate && pip install -r requirements.txt   # ครั้งแรกเท่านั้น
robot -d results/run1 -v BROWSER:headlesschrome pizza_scenario.robot
```

ถ้าต้องการรันเทสต์ทุกไฟล์พร้อมกัน ใช้ `robot .` แทน

## cypress/ (Part 4, Cypress)

ใช้ Cypress 16.3.0 ไฟล์ `cypress/e2e/web-form.cy.js` ทดสอบ web form และ `cypress/e2e/pizza-scenario.cy.js` รัน QP-01 โดยการ login ของ admin เป็น custom command `cy.adminLogin` ใน `cypress/support/commands.js` โฟลเดอร์ `cypress/videos` และ `cypress/screenshots` เก็บวิดีโอและ screenshot ของการรันเต็มหนึ่งรอบ

รันทุก spec จากโฟลเดอร์ `cypress`:

```bash
cd cypress
npm install   # ครั้งแรกเท่านั้น
npx cypress run --browser chrome
```
