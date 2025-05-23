# BR360: Bookmarklet for Risco 360
`ㅤ/ˈbre.ɡo/ ㅤ`

There is a common issue with the insurance provider and their minor medical expenses service, they don't let you know how much you have expended per user, making it hard to keep track of it and know how much you still can reimburse.

That's why I reverse engineered it and created this script that gets the data the same way they do and serves it back to you in the way you want it: split per user instead of as a whole.

## Usage

To use it just drag our bookmarklet: <a href="javascript:%22use%20strict%22;void%20function(){const%20a=ontdc(80,obtenerValorParametro(%22tkn%22),document.getElementById(%22usrSs%22).innerText),b=a.reduce((a,b)=%3E(a[b.NameAfectado]=a.hasOwnProperty(b.NameAfectado)%3Fa[b.NameAfectado]+b.TTReembolso:b.TTReembolso,a),{}),c=Object.entries(b).map(a=%3E{let[b,c]=a;return`${b}:%20$${c.toFixed(2)}`}).join(%22\n%22);alert(c)}();">BR36O</a> to your browser's bookmark bar and click on it from Risco 360 website. You will see a new window pop-up with the amount per user.

## How it works

If you want to know what does the script do exactly, here is a less cryptic version of it.

```javascript
const records = ontdc( // This general function retrieves all data they need depending on the parameters
  80, // Internal Code for reimbursement requests
  obtenerValorParametro('tkn'), // Your auth token
  document.getElementById("usrSs").innerText // Your user ID
);

const groupedByUser = records.reduce((totals, record)=> {
  totals.hasOwnProperty(record.NameAfectado) // Checks if the name already exists
    ? totals[record.NameAfectado] = totals[record.NameAfectado] + record.TTReembolso // So it adds the current value to the accumulated
    : totals[record.NameAfectado ] = record.TTReembolso; return totals; },{}); // Otherwise, it creates the new record

const prettyPrint = Object.entries(groupedByUser).map(([name, amount]) => `${name}: $${ amount.toFixed(2) }`).join('\n'); // Stores the data with the format: "NAME: $AMOUNT" with a new line in between

alert(prettyPrint); // Prints the values out
```

In general terms, it uses the same function they use internally to get your records, then puts it into a hashmap to get the totals per user and then sends a human readable version of it to you.

## Credits

It all got minified using: https://chriszarate.github.io/bookmarkleter/
