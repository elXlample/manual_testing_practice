# Test-design techniques for Manual Testing

1. Equivalence classes & boundary values analysis

2. Decision Table Testing

3. Error Guessing

4. Pairwise testing

5. State diagram

## Equivalence classes & boundary values analysis

For example we have a function *on the hood* that takes one date and identifies the next one (day+1 date):

```python
def next_date(date:datetime):
 """here some logic"""
    return next_date
```

We need to split all the dates using following logic:

    by the month: M1,M2,M3,M4,M5

    where M1 - 31-days months, M2 - 30-days months, M3 - February in lapse year, M4 - February in normal year, M5 - December

Using boundary values analysis we should check:

![Scale example](..assets/scale.jpg)

    -lower boundary - 1 (0)

    -lower boundary (1)

    -middle value (15)

    -upper boundary (28,29,30 or 31)

    -upper boundary + 1 (29,30,31,32)

Same for the months (0,01,6,12,13) and years (0000,0001,1013,2026,2027)

Here we have some **decision table**:

| Rule number | Month            | Year         | Day       | Expected result              |
| ----------- | ---------------- | ------------ | --------- | ---------------------------- |
| 1           | M1 = 01          | 0001         | 31        | 01, M + 1                    |
| 2           | M2               | 2026         | 30        | 01, M + 1                    |
| 3           | M3               | any valid    | 29        | 01, M + 1                    |
| 4           | M4               | any valid    | 28        | 01, M + 1                    |
| 5           | M5 = 12          | any valid    | 31        | 01, M = 01, Y + 1            |
| 6           | all except M5    | any valid    | 01        | 02, M                        |
| 7           | any valid        | 0000 or 2027 | any valid | error message('wrong year')  |
| 8           | 00 or 13         | any valid    | any valid | error message('wrong month') |
| 9           | any              | any valid    | 0 or 32   | error message('wrong day')   |
| 10          | all except M1,M5 | any valid    | 31        | error message('wrong day')   |
| 11          | M3 or M4         | any valid    | 30        | error message('wrong day')   |
| 12          | M4               | any valid    | 29        | error message('wrong day')   |

  

## Decision table testing

For example, we have a list of *conditions*:

 c1: account is exist (y/n)

c2: password is correct (y/n)

c3: account is blocked (y/n)

c4: active subscription (y/n)

And the following *actions*:

a1: successful log-in

a2: error "account does not exist"

a3: error "wrong password"

a4: error "account is blocked"

a5: limited log-in, subscription suggestion

**Decision table** for that example:

| Rule number | c1  | c2  | c3  | c4  | Result |
|:-----------:|:---:|:---:|:---:|:---:|:------:|
| R1          | yes | yes | no  | yes | **a1** |
| R2          | no  | -   | -   | -   | **a2** |
| R3          | yes | no  | -   | -   | **a3** |
| R4          | yes | yes | yes | -   | **a4** |
| R5          | yes | yes | no  | no  | **a5** |

## Error Guessing

For example, we need to validate a username field on the website. Since we already have some experience of testing different forms, we can guess the vulnarabilities of this current form. We can check the following inputs:

    1. Flie with .css extension

    2. SQL-injection

    3. XSS (<script> ... </script>)

    4. html-tags

## Pairwise testing

This test-design technique is used when we have a lot of parameters and their combinations. For example, we need to test our application on different devices (Desktop, Mobile, Smart TV) in most popular browsers (Chrome, Safari, Edge) using different quality settings (SD, HD, 4K) and subscription type (Free, Standard, Premium).
![Content table example](..assets/variants.jpg)   

Using an *exhaustive search* a tester will need to perform 3x3x3x3 = 81 test-cases.

But if he uses pairwise testing, it will take only 9 test-cases:

| number | Device   | Browser | Quality | Subscription |
|:------:|:--------:|:-------:|:-------:|:------------:|
| *TC-1* | Desktop  | Chrome  | SD      | Free         |
| *TC-2* | Desktop  | Safari  | HD      | Standard     |
| *TC-3* | Desktop  | Edge    | 4K      | Premium      |
| *TC-4* | Mobile   | Safari  | 4K      | Free         |
| *TC-5* | Mobile   | Edge    | SD      | Standard     |
| *TC-6* | Mobile   | Chrome  | HD      | Premium      |
| *TC-7* | Smart TV | Edge    | HD      | Free         |
| *TC-8* | Smart TV | Chrome  | 4K      | Standard     |
| *TC-9* | Smart TV | Safari  | SD      | Premium      |

## State diagram

We have 2 type of entities in this approach: states and actions we need to take to trigger the transition/error message

For example, let's look at the log-in page:

    ![State diagram example](..assets/state_diagram.jpg)   

We can create test-cases using this diagram:

| #      | Initial state      | Action                          | Status  | Expected result                                                   |
|:------:|:------------------:|:-------------------------------:| ------- | ----------------------------------------------------------------- |
| *TC-1* | Log-in Page Opened | enter valid email and password  | Success | User logged-in successfully and transitioned to the Home Page     |
| *TC-2* | Log-in Page Opened | enter invalid email or password | Failure | "Invalid credentials" error appears. No transition between pages. |
