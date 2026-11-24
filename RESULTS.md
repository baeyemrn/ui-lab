# UI lab results
Tool parts that I chose: Part 3 (Robot Framework) and Part 4 (Cypress)
## Part 0
- Step 0.1, Chrome version: 158.0.8127.37 (arm64) (first 156.0.8078.82; Chrome updated itself during the lab, so Steps 0.1 to 0.3 were repeated)
- Step 0.3, ChromeDriver version: ChromeDriver 158.0.8127.37 (b8c2be851f62f6a394ec57955d7f31335d7cb6d4-refs/branch-heads/8127@{#1692})
- Step 0.4, locator of Pizza, Please!: `button[name="pizza-please"]` (CSS)
- Step 0.4, locator of Excluded tools: `select` (CSS, the only select on the page)
- Step 0.4, locator of Must be vegetarian: `#default-checkbox` (CSS)
- Step 0.4, locator of Custom Pizza Name: `input#pizza-name` (CSS; `#pizza-name` alone finds 2 elements after Pizza, Please!)
- Step 0.5, pizza name, tool and calories of the run by hand: Lab Pizza 20261124-2049, tool Scissors, calories per slice 375

| Step | Actual result | Pass or fail |
|------|---------------|--------------|
| 1    | Heading reads "Looking to break out of your pizza routine?" | Pass |
| 2    | Max Calories per Slice, Excluded tools, Must be vegetarian and Custom Pizza Name appeared | Pass |
| 3    | Fields show 600, Knife, vegetarian ticked, Lab Pizza 20261124-2049 | Pass |
| 4    | Answer shows Name: Lab Pizza 20261124-2049, Tool: Scissors (not Knife), Calories per slice: 375 (600 or fewer) | Pass |
| 5    | Clicking Love it! without login shows "Please log in first." | Pass |
| 6    | Signed in as admin/admin, page shows "Latest pizza recommendations" | Pass |
| 7    | List line: Lab Pizza 20261124-2049 (tool=Scissors, ingredients_number=5), same tool as step 4 | Pass |

## Part 3: Robot Framework
- robot --version: Robot Framework 7.5.1 (Python 3.14.2 on darwin)
- Step 3.2, failure message for the wrong title: Title should have been 'Web Form' but was 'Web form'.
- Step 3.8, run 1, result, elapsed time and pizza name: PASS (1 test, 1 passed, 0 failed), 00:00:07.593, Lab Pizza 20261124-211608
- Step 3.8, run 2, result, elapsed time and pizza name: PASS (1 test, 1 passed, 0 failed), 00:00:06.586, Lab Pizza 20261124-211615
## Part 4: Cypress
- node --version, npm --version and npx cypress version: v22.20.0, 10.9.3, Cypress package 16.2.1 / binary 16.2.1 (Electron 41.7.0, bundled Node 24.15.0)
- Step 4.3, failure message for the wrong title: Timed out retrying after 10000ms, + expected - actual, -'Web form' +'Web Form'
- Step 4.6, tool and calories in the answer of POST /api/pizza:
- Step 4.7, the line of the admin list that shows your pizza:
- Step 4.8, run 1, result, duration and pizza name:
- Step 4.8, run 2, result, duration and pizza name:
- Step 4.9 (optional), browser and result:

## Part B
1.
2.
3.
4.
5.
6.
7.