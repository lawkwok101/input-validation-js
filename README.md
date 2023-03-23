# JS: How to only accept the currency format in an input?
Let's say we have an banking website and we want to only accept the currency format on an input element (i.e. only 2 decimal places such as `123.50`). We can use JavaScript along with regular expressions (regex) to accomplish this. You can replace the regex expression to suit your requirements.

## Code:

### HTML
The decimal input mode activates the number-only keyboard on mobile devices.
```HTML
<input inputmode="decimal">
```

### JS
```javascript
function validateInput(e) {

  // This is the "destructuring assignment" syntax. It is equivalent to: const value = e.target.value;
  const {value} = e.target;
  
  // Go to regex101.com for details about this regular expression: "/^\d{0,3}\.?\d{0,2}$/"
  const regex = '/^\d{0,3}\.?\d{0,2}$/';
  const isValid = regex.test(value);

  // If the user enters an invalid character, remove that character.
  if (!isValid) {
    e.target.value = value.slice(0, -1);
  }
}

document.addEventListener('input', validateInput);
```

## Why not just use HTML?
1. You would think that adding the `type="number"` attribute to the input element would ignore any characters that aren't numbers. However, all the browser does it add the `:invalid` pseudoclass to the input element so the browser can apply the CSS for invalid inputs.

2. It is usually better to provide inline validation as soon as the error arises instead of waiting for the user to submit a form [^1]. The user may be confused about why they are even able to enter letters into a field that is meant for numbers. 

3. If there is no submit button (common on websites that process input as you type), your browser won't even throw up the default tooltip.

## Notes

If you are using a debounce function, make sure you don't apply it to the event listener that calls `validateInput()`. Otherwise:
- The user will be able to enter invalid characters if they type fast enough.
- Any invalid characters will be visible to the user as long as your debounce delay is. This will make your website feel slow.
  
## Footnotes:
[^1]: [https://www.nngroup.com/articles/errors-forms-design-guidelines/](https://www.nngroup.com/articles/errors-forms-design-guidelines/)
