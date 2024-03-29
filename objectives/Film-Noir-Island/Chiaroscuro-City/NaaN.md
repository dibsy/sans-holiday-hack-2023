## Objective


## Solution

#### NaN concepts 0
- NaN (Not a Number) is a special value representing missing or undefined Python data.

#### NaN concepts 1
- ```float(str)``` can process a string "NaN" without any error
- When added or multiplied or any other operation is performed with the ```nan``` it is nan which wins
- Some operations with NaN
```python
small = float("NaN")
big = float(999.999)

print(big>small)
print(small>big)
print(small==big)
print(small -1)
print(small +1)
print(0*small)
print(0/small)
print(small/0)
```
```output
False
False
False
nan
nan
nan
nan
Traceback (most recent call last):
  File "/home/cg/root/65988fb9cd2b1/main.py", line 12, in <module>
print(small/0)
ZeroDivisionError: float division by zero
```
#### NaN concepts 2

- When float converts ```NaN``` it becomes ```nan```
- NaN conversion effect
```python
boo = float("NaN")
if boo == "NaN":
    print("Got a NaN")
else:
    print("There is no NaN? What is the the value of boo?")
    print(boo)
```
```
There is no NaN? What is the the value of boo?
nan
```

#### NaN injection example PoC 1
- The code below is supposed to store 5 unique numbers
- We can enter ```NaN``` 5 times and at the end of the execution the entire list will have 5 ```nan``` values
- Any number repeated will result in ```Number already entered ...```. However for ```NaN``` input, we can bypass the check.
```python3
def get_unique_numbers():
    unique_numbers = set()

    while len(unique_numbers) < 5:
        try:
            user_input = float(input(f"Enter unique number {len(unique_numbers) + 1}: "))
            if user_input in unique_numbers:
                print("Number already entered. Please enter a unique number.")
            else:
                unique_numbers.add(user_input)
        except ValueError:
            print("Invalid input. Please enter a valid number.")

    return list(unique_numbers)

if __name__ == "__main__":
    unique_numbers_list = get_unique_numbers()
    print("You entered the following unique numbers:", unique_numbers_list)
```
```
Enter unique number 1: 4
Enter unique number 2: 4
Number already entered. Please enter a unique number.
Enter unique number 2: NaN
Enter unique number 3: NaN
Enter unique number 4: NaN
Enter unique number 5: NaN
You entered the following unique numbers: [nan, 4.0, nan, nan, nan]
```

#### NaN injection example PoC 2
- The code below stores 5 numbers and returns the max and the min value
- If we add a mix of various numbers including a "NaN", both the higest and lowest will be ```nan```
```python
def get_user_inputs():
    numbers = []
    for i in range(5):
        try:
            user_input = float(input(f"Enter number {i + 1}: "))
            numbers.append(user_input)
        except ValueError:
            print("Invalid input. Please enter a valid number.")
            i -= 1  # Decrement the loop counter to re-enter the current input

    return numbers

if __name__ == "__main__":
    user_numbers = get_user_inputs()

    if user_numbers:
        highest_number = max(user_numbers)
        lowest_number = min(user_numbers)

        print("Entered numbers:", user_numbers)
        print("Highest number:", highest_number)
        print("Lowest number:", lowest_number)
```
```
Enter number 1: NaN
Enter number 2: -11
Enter number 3: 0
Enter number 4: 3333
Enter number 5: 44
Entered numbers: [nan, -11.0, 0.0, 3333.0, 44.0]
Highest number: nan
Lowest number: nan
```
### Challenge Solution
#### Dump code via error stacktrace

- Trigger an error that will dump the code
```
{"play":"0x5,6,7,8,9"}
```

#### Vulnerable code analysis
- Inject NaN
- Bypass the unique card check
- ```max()``` and ```min()``` returns ```nan``` is all cases
```python3
# Error in function named play_cards:
....
        for row in reader:
            for n in row:
                n = float(n) #----------> Inject NaN here
                if is_valid_whole_number_choice(n) and n not in [x['num'] for x in player_cards]:
                    player_cards.append({
                        'owner': 'p',
                        'num': n
                    })
            break

        #-----------> Bypass the check here
        if len(player_cards) != 5:
            return jsonify({
                "request": False,
                "data": f"Requires 5 unique values but was given \"{csv_card_choices}\""
            })
....
        
        maxItem = False
        minItem = False

        #-----------> NaN magic here
        if bool(len(all_cards)):
            maxItem = max(all_cards, key=lambda x: x['num'])
            minItem = min(all_cards, key=lambda x: x['num'])

....
```

#### Forge for the win
- We will use Burp Proxy to solve the challenge as there is a client side validation in place
- We intercept the data and send JSON data with NaN for all the cards number
```json
{"play":"NaN,NaN,NaN,NaN,NaN"}
```
- We keep doing it till our score reaches 10 and we will find the challenge completion hash in the json response
```json
{
   "data":{
      "conduit":{
         "hash":"a2a6c9bcc95608962d98d885e847c08e36908e00048cedf000fd259bd02e6f61",
         "resourceId":"b5187f3e-8cd5-48c3-9796-001f332ea558"
      },
     ......
}
```

#### Reference
- https://www.tenable.com/blog/python-nan-injection
