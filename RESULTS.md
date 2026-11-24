# UI lab results
Tool parts that I chose: Part 3 (Robot Framework) และ Part 4 (Cypress)
## Part 0
- Step 0.1, Chrome version: 158.0.8127.37 (arm64) (ครั้งแรกเป็น 156.0.8078.82 แต่ Chrome อัปเดตตัวเองระหว่างทำแล็บ จึงทำ Step 0.1 ถึง 0.3 ซ้ำ)
- Step 0.3, ChromeDriver version: ChromeDriver 158.0.8127.37 (b8c2be851f62f6a394ec57955d7f31335d7cb6d4-refs/branch-heads/8127@{#1692})
- Step 0.4, locator of Pizza, Please!: `button[name="pizza-please"]` (CSS)
- Step 0.4, locator of Excluded tools: `select` (CSS, เป็น select เพียงตัวเดียวในหน้า)
- Step 0.4, locator of Must be vegetarian: `#default-checkbox` (CSS)
- Step 0.4, locator of Custom Pizza Name: `input#pizza-name` (CSS; ถ้าใช้ `#pizza-name` อย่างเดียว หลังกด Pizza, Please! จะเจอ 2 element)
- Step 0.5, pizza name, tool and calories of the run by hand: Lab Pizza 20261124-2049, tool เป็น Scissors, calories per slice เท่ากับ 375

| Step | Actual result | Pass or fail |
|------|---------------|--------------|
| 1    | หัวข้อแสดงข้อความ "Looking to break out of your pizza routine?" | Pass |
| 2    | ช่อง Max Calories per Slice, Excluded tools, Must be vegetarian และ Custom Pizza Name ปรากฏขึ้น | Pass |
| 3    | ช่องต่างๆ แสดง 600, Knife, ติ๊ก vegetarian แล้ว และชื่อ Lab Pizza 20261124-2049 | Pass |
| 4    | คำตอบแสดง Name: Lab Pizza 20261124-2049, Tool: Scissors (ไม่ใช่ Knife), Calories per slice: 375 (ไม่เกิน 600) | Pass |
| 5    | กด Love it! โดยไม่ login แล้วขึ้นข้อความ "Please log in first." | Pass |
| 6    | login ด้วย admin/admin แล้วหน้าแสดง "Latest pizza recommendations" | Pass |
| 7    | บรรทัดในรายการ: Lab Pizza 20261124-2049 (tool=Scissors, ingredients_number=5) tool ตรงกับ step 4 | Pass |

## Part 3: Robot Framework
- robot --version: Robot Framework 7.5.1 (Python 3.14.2 on darwin)
- Step 3.2, failure message for the wrong title: Title should have been 'Web Form' but was 'Web form'.
- Step 3.8, run 1, result, elapsed time and pizza name: ผ่าน (1 test, 1 passed, 0 failed), ใช้เวลา 00:00:07.593, Lab Pizza 20261124-211608
- Step 3.8, run 2, result, elapsed time and pizza name: ผ่าน (1 test, 1 passed, 0 failed), ใช้เวลา 00:00:06.586, Lab Pizza 20261124-211615
## Part 4: Cypress
- node --version, npm --version and npx cypress version: v22.20.0, 10.9.3, Cypress 16.3.0 (ตอนแรกติดตั้ง 16.2.1 แต่เมื่อใช้กับ Chrome 158 แล้ว `cypress run` พังด้วยข้อความ "Error: * is not a valid URL" ตอนเริ่ม spec ที่สอง แม้จะเป็น spec เปล่าสองไฟล์ก็ตาม จึงอัปเกรดเป็น 16.3.0 ซึ่งแก้ปัญหานี้ได้)
- Step 4.3, failure message for the wrong title: Timed out retrying after 10000ms, + expected - actual, -'Web form' +'Web Form'
- Step 4.6, tool and calories in the answer of POST /api/pizza: tool เป็น Scissors, calories เท่ากับ 575 (พิซซ่า Lab Pizza 20261124-213915)
- Step 4.7, the line of the admin list that shows your pizza: Lab Pizza 20261124-214155 (tool=Pizza cutter, ingredients_number=8)
- Step 4.8, run 1, result, duration and pizza name: All specs passed! (2 tests, 2 passing), ใช้เวลา 00:10 (pizza-scenario 00:08, web-form 00:01), Lab Pizza 20261124-215039 (tool=Scissors) รอบนี้มีพิซซ่าของผู้ใช้คนอื่นเป็นบรรทัด newest อยู่เหนือพิซซ่าของเรา
- Step 4.8, run 2, result, duration and pizza name: All specs passed! (2 tests, 2 passing), ใช้เวลา 00:10 (pizza-scenario 00:08, web-form 00:01), Lab Pizza 20261124-215056 (tool=Pizza cutter, เป็นบรรทัด newest)
- Step 4.9 (optional), browser and result: ไม่ได้ทำ เพราะ `npx cypress info` เจอแค่ Chrome ในเครื่องนี้

## Part B
1. ช่องตัวเลขสามช่องของ QuickPizza (Max Calories per Slice, Min Number of Toppings และ Max Number of Toppings) ใช้ id `floating_filled` เหมือนกันทั้งหมด ใน Step 0.4 คำสั่ง `$$("#floating_filled").length` ได้ผลเป็น 3 locator ที่ใช้ id นี้จึงบอกไม่ได้ว่าหมายถึงช่องไหน และจะได้ช่องแรกที่เจอเสมอ เทสต์จึงเริ่มหาจากข้อความของ label ที่มองเห็นได้ แล้วไปยัง input ที่อยู่ข้างกัน คือ `xpath://label[contains(.,'Max Calories')]/../input` ใน Robot Framework และ `cy.contains('label', 'Max Calories per Slice').siblings('input')` ใน Cypress ผมเสนอให้หน้าเว็บกำหนด id ที่ไม่ซ้ำกันให้แต่ละช่อง เช่น `id="max-calories-per-slice"` (และใส่ `for` ที่ตรงกันใน label) หรือใส่ attribute สำหรับเทสต์ เช่น `data-testid="max-calories"` ผู้ทดสอบจะได้ใช้ locator แบบ id ที่สั้น เจอ element เดียวแน่นอน ไม่พังเมื่อข้อความของ label หรือ layout เปลี่ยน และอ่านง่ายกว่า XPath

2. การตรวจนี้เป็น consistency oracle คือเปรียบเทียบผลลัพธ์สองจุดของระบบเดียวกัน ได้แก่ tool ในคำแนะนำพิซซ่ากับ tool ในรายการของ admin แทนที่จะเทียบกับค่าที่รู้ล่วงหน้า QuickPizza สุ่มพิซซ่าให้ เทสต์จึงรู้ไม่ได้ว่า tool ที่ถูกต้องคืออะไร แต่รู้ว่าทั้งสองจุดต้องตรงกัน การตรวจนี้จะจับข้อผิดพลาดที่ server บันทึกพิซซ่าคนละอันกับที่ส่งให้ลูกค้าได้ เช่น บั๊กที่บันทึก tool ของ request ก่อนหน้า หรือแทน tool ด้วยค่า default ทำให้รายการของ admin แสดง `(tool=Knife, ...)` สำหรับพิซซ่าที่ลูกค้าเห็นว่าเป็น Scissors และยังจับกรณีที่รายการของ admin ดึงข้อมูลเก่ามาแสดง ทำให้พิซซ่าใหม่มีรายละเอียดผิดได้ด้วย

3. เพิ่มการตรวจโดยเทียบส่วนผสม (ingredients) ทุกอย่างของพิซซ่าที่ได้กับรายการส่วนผสมที่เป็นเนื้อสัตว์และอาหารทะเล (เช่น Pepperoni, Ham, Bacon, Salami, Anchovies หรือ Tuna) และให้เทสต์ fail ถ้าเจอส่วนผสมใดในรายการนั้น ใน Cypress คำตอบของ `POST /api/pizza` ที่ Step 4.6 ดักไว้แล้วมีทั้งรายการ ingredients และฟิลด์ `"vegetarian": true` เทสต์จึงตรวจได้ว่า `response.body.vegetarian` เป็น true และไม่มีส่วนผสมใดอยู่ในรายการเนื้อสัตว์ หน้าเว็บเองไม่ได้แสดงว่าพิซซ่าเป็นมังสวิรัติหรือไม่ เทสต์ที่อ่านจากหน้าเว็บอย่างเดียวจึงต้องได้รายการส่วนผสมที่ไม่ใช่มังสวิรัติจากแหล่งอื่น ซึ่งควรมาจาก specification หรือข้อมูลส่วนผสมของ QuickPizza เอง ไม่ใช่รายการที่ผู้ทดสอบเดาขึ้นมา

4. Robot Framework จะรอเฉพาะจุดที่เทสต์สั่งให้รอ ผมใช้ `Wait Until Element Is Visible css:h1`, `Wait Until Page Contains Element xpath://option[.='Knife']`, `Wait Until Element Contains id:recommendations Name:` และรอ `id:svelte-announcer` ก่อน login admin โดยตั้ง `timeout=10s` ไว้ที่ library ส่วน Cypress รอเอง เพราะทุก `cy.get` และ `should` จะลองซ้ำได้นานถึง `defaultCommandTimeout` (10 วินาที) ผมจึงไม่ต้องเขียนคำสั่งรอหัวข้อ รายการ tool หรือคำตอบเลย มีแค่หน้า admin ที่ยังต้องใช้ `cy.get('#svelte-announcer').should('exist')` และใช้ `cy.wait('@pizza')` รอ request การเขียน QP-01 ด้วย Cypress ทำได้เร็วกว่า เพราะคำสั่งแบบ chain สั้น และ `cy.intercept` ให้ค่า tool กับ calories มาโดยไม่ต้องแยกข้อความเอง ในขณะที่ Robot Framework ต้องเขียน keyword เพิ่ม เช่น `Get Recommended Tool` ที่ใช้ `Remove String` ความเร็วของทั้งสองใกล้เคียงกัน QP-01 ใช้เวลา 7.6 และ 6.6 วินาทีในการรัน Robot แบบ headless สองรอบ และประมาณ 8 วินาทีต่อรอบใน Cypress ซึ่งบันทึกวิดีโอไปด้วย `log.html` ของ Robot เป็นรายงานที่ละเอียดที่สุด แสดงทุก keyword ข้อความของแต่ละ keyword (เช่น `Content of text field ... is '600'.`) และ screenshot ณ step ที่ถ่าย ส่วนวิดีโอของ Cypress แสดง Command Log คู่กับหน้าเว็บ ทำให้เห็นได้ง่ายว่าในการรันเกิดอะไรขึ้น

5. ผมจะเลือก Cypress สำหรับเทสต์ UI อัตโนมัติของ Project 2 เหตุผลข้อแรก การลองซ้ำอัตโนมัติของทุกคำสั่งค้นหาและคำสั่งตรวจ ทำให้แทบไม่ต้องเขียนคำสั่งรอเลย และการรันก็ยังเสถียร การรันเต็มสองรอบใน Step 4.8 ผ่านทั้งคู่ ซึ่งตรงกับกฎของ Project 2 ที่ให้รอ element และห้ามหยุดรอแบบกำหนดเวลาตายตัว เหตุผลข้อสอง Cypress สร้างหลักฐานที่ Project 2 ต้องการได้เอง คือวิดีโอของแต่ละ spec และ screenshot ด้วย `cy.screenshot()` และ `cy.intercept` ยังทำให้เทสต์ตรวจได้ทั้ง request และคำตอบของ server นอกเหนือจากหน้าเว็บ อย่างใน Step 4.6 ข้อเสียที่เจอคือ Cypress ขึ้นกับเวอร์ชันของเบราว์เซอร์ Cypress 16.2.1 พังด้วย `Error: * is not a valid URL` บน Chrome 158 จนต้องอัปเกรดเป็น 16.3.0 กลุ่มจึงควรล็อกและตรวจเวอร์ชันของ Cypress ตั้งแต่เนิ่นๆ

6. (Part 3) ถ้า QuickPizza เปลี่ยน locator ของช่อง calories ผมจะแก้เพียงบรรทัดเดียว คือตัวแปร `${MAX CALORIES}` ในส่วน Variables ของ `robot/resources/quickpizza.resource` keyword `Turn On Advanced` และ `Fill In The Pizza Request` ใช้ตัวแปรนี้ทั้งสามที่ (การตรวจว่ามองเห็น, `Input Text` และ `Textfield Value Should Be`) จึงได้ locator ใหม่ทันที `pizza_scenario.robot` ไม่มี locator อยู่เลย มีแค่การเรียก `Fill In The Pizza Request    600    Knife    ${name}` ด้วยภาษาของ test case ไฟล์เทสต์จึงไม่ต้องแก้อะไรเลย และเทสต์อื่นทุกตัวที่ import resource file นี้ก็จะได้รับการแก้ไขด้วยการเปลี่ยนบรรทัดเดียวนี้เช่นกัน

7. (Part 4) QuickPizza รันอยู่บนหลาย server และ cookie จะผูกเบราว์เซอร์หนึ่งไว้กับ server เดียว รายการของ admin แสดงเฉพาะพิซซ่าของ server นั้น Cypress ล้าง cookie ก่อนเริ่มแต่ละเทสต์ ถ้าแยก step 6 และ 7 เป็นเทสต์ที่สอง เทสต์นั้นจะเริ่มโดยไม่มี cookie และอาจถูกส่งไปอีก server หนึ่ง ซึ่งรายการของ admin ไม่มีพิซซ่าที่เทสต์แรกสร้างไว้ ทำให้ `cy.contains('li', pizzaName)` fail หลังรอครบ 10 วินาที ผมเห็นผลแบบเดียวกันตอนลองด้วยมือ คือรายการของ admin ที่เปิดใน Chrome ของผมเองไม่มี Lab Pizza จากการรัน Cypress เลย นอกจากนี้เทสต์ที่สองจะไม่มีค่าที่ step 7 ต้องใช้ เพราะ `pizzaName` เป็นตัวแปรภายในเทสต์แรก และ alias `@tool` จะถูกล้างระหว่างเทสต์ จึงไม่รู้ว่าต้องหาพิซซ่าตัวไหนและ tool อะไร
