+++
title = 'Password Generator'
date = 2024-05-02T16:14:40-04:00
draft = false
+++

The basics of this was a friend has a website and wanted to have a page that would generate a strong password.

**Nothing is stored anywhere** the page allows you to click "GET IT" button to add it to your clipboard and use, storing however passwords are saved per user.

This also allows the user to select the length of the password via a slider or + - buttons and the ability to add or remove character sets as desired.

Also if the browser does not support mordern browswer apis that is also handled.

[Live Example](http://www.worldwired.net/p)


```javascript
  var newOp = "";
  var cbLower = document.getElementById('cbLower')
  var cbUpper = document.getElementById('cbUpper')
  var cbNumber = document.getElementById('cbNumber')
  var cbSymbol = document.getElementById('cbSymbol')
  var slider = document.getElementById('sLength')
  var index = 16
  
  cbLower.addEventListener('change', event => {
   newOp = generateOp();
   document.getElementById("op").value = newOp;
  })
  
  cbUpper.addEventListener('change', event => {
   newOp = generateOp();
   document.getElementById("op").value = newOp;
  })
  
  cbNumber.addEventListener('change', event => {
   newOp = generateOp();
   document.getElementById("op").value = newOp;
  })
  
  cbSymbol.addEventListener('change', event => {
   newOp = generateOp();
   document.getElementById("op").value = newOp;
  })
  
  slider.addEventListener('change', event => {
   index = 0;
   op = 0;
   index = event.target.value;
   label = document.getElementById('lLength');
   label.innerHTML = "Length: " + index;
   refresh();
  })
  
  var newOp = generateOp();
  
  function isOnlyCbSymbolChecked(checkboxes) {
   var checkedCount = checkboxes.reduce((acc, checkbox) => acc + (checkbox ? 1 : 0), 0);
   return checkboxes[3] && checkedCount === 1;
  }
  
  function generateOp() {
   var checkboxes = [cbUpper.checked, cbLower.checked, cbNumber.checked, cbSymbol.checked];
   var binaryNumber = parseInt(checkboxes.map((checkbox) => checkbox ? '1' : '0').join(''), 2);
   var opLength = index;
   var charsetLower = "abcdefghijkmnopqrstuvwxyz";
   var charsetUpper = "ABCDEFGHJKLMNPQRSTUVWXYZ";
   var charsetNumber = "123456789";
   var charsetSymbol = "!$/&()-.,?";
   var op = "";
   var useSet = getTotalFlags();
   var flagLower = 0;
   var flagUpper = 0;
   var flagNumber = 0;
   var flagSymbol = 0;
   var flagTotal = 0;
   var setLength = 0;
   var max = getRollsArray();
   var maxInt = Math.max(...max);
   var randint = 0;
   
   if (max.length == 0) {
    op = "Um.... No.";
    return op;
   }
   
   var onlyCbSymbolChecked = isOnlyCbSymbolChecked(checkboxes);
   
   do {
    randint = Math.floor(Math.random() * maxInt + 1);
    if (max.includes(1) == true) {
     if (cbLower.checked == true && op.length < opLength && randint == 1) {
      setLength = charsetLower.length;
      op += charsetLower.charAt(Math.floor(Math.random() * setLength));
      flagLower = 1;
     }
    }
    if (max.includes(2) == true) {
     if (cbUpper.checked == true && op.length < opLength && randint == 2) {
      setLength = charsetUpper.length;
      op += charsetUpper.charAt(Math.floor(Math.random() * setLength));
      flagUpper = 1;
     }
    }
    if (max.includes(3) == true) {
     if (cbNumber.checked == true && op.length < opLength && randint == 3) {
      setLength = charsetNumber.length;
      op += charsetNumber.charAt(Math.floor(Math.random() * setLength));
      flagNumber = 1;
     }
    }
    if (max.includes(4) == true) {
     if (
      cbSymbol.checked == true && op.length < opLength && (onlyCbSymbolChecked || (randint == 4 && op.length > 0))) {
      setLength = charsetSymbol.length;
      op += charsetSymbol.charAt(Math.floor(Math.random() * setLength));
      flagSymbol = 1;
     }
    }
    
    flagTotal = flagLower + flagUpper + flagNumber + flagSymbol;
    
    if (op.length == opLength && flagTotal == useSet) {
     return op;
    } else if (op.length < opLength && flagTotal == useSet) {
     flagTotal = 0;
    } else if (op.length == opLength && flagTotal < useSet) {
     op = "";
     flagLower = 0;
     flagNumber = 0;
     flagSymbol = 0;
     flagUpper = 0;
     flagTotal = 0;
    }
   } while (op.length <= opLength);
  }
  
  document.getElementById("op").value = newOp;
   var copyOpButton = document.querySelector('#getit');
   copyOpButton.addEventListener('click', function (event) {
    var copyOpText = document.querySelector('#op');
    var text = document.getElementById("op").value;
    if (!navigator.clipboard) {
     copyOpText.focus();
     copyOpText.select();
     document.execCommand('copy');
     copyOpText.blur();
    } else {
     navigator.clipboard.writeText(text).then(() => { }, (err) => {
      console.log(err);
     });
    }
   });
   
   function refresh() {
    newOp = generateOp();
    document.getElementById("op").value = newOp;
   }
   
   function getTotalFlags() {
    var count = 0;
    if (cbLower.checked == true) {
     count += 1;
    }
    if (cbUpper.checked == true) {
     count += 1;
    }
    if (cbNumber.checked == true) {
     count += 1;
    }
    if (cbSymbol.checked == true) {
     count += 1;
    }
    return Math.abs(count);
   }
   
   function getRollsArray() {
    var arr = [];
    if (cbLower.checked == true) {
     arr.push(1);
    }
    if (cbUpper.checked == true) {
     arr.push(2);
    }
    if (cbNumber.checked == true) {
     arr.push(3);
    }
    if (cbSymbol.checked == true) {
     arr.push(4);
    }
    return arr;
   }
   
   function stepUp() {
    slider.value = parseInt(slider.value) + 1;
    slider.dispatchEvent(new Event('change'));
   }
   
   function stepDown() {
    slider.value = parseInt(slider.value) - 1;
    slider.dispatchEvent(new Event('change'));
   }
   ```

Later, I wanted to make it a bit better and I re-did it in python.
This implementation includes regex. Ideally this would be updated in the future to include passing in arguments for length and numbers for different character sets.


```python
import re
import secrets
import string

# default values
def generate_password(length=16, nums=1, special_chars=1, uppercase=1, lowercase=1):

    # Define the possible characters for the password
    letters = string.ascii_letters
    digits = string.digits
    symbols = string.punctuation

    # Combine all characters
    all_characters = letters + digits + symbols

    while True:
        password = ''
        # Generate password
        for _ in range(length):
            password += secrets.choice(all_characters)
        
        constraints = [
            (nums, r'\d'),
            (special_chars, fr'[{symbols}]'),
            (uppercase, r'[A-Z]'),
            (lowercase, r'[a-z]')
        ]

        # Check constraints        
        if all( #this is a generator, converted from a list comprehension
            constraint <= len(re.findall(pattern, password))
            for constraint, pattern in constraints
        ):
            break
    
    return password

if __name__ == "__main__":
# we can pass any argument we want since it has default values
# ex: generate_password(20, 2, 2, 2, 2)    
    new_password = generate_password()
    print("Generated Password: ", new_password)
```