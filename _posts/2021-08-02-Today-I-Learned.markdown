---
layout: post
title:  "Today I Learned: Javascript polyfills, Array.prototype.at(), Math.trunc() and Object.defineProperty()"
date:   2021-08-02 21:51:00 +0530
---
Javascript finally has an option for accessing Array items by relative index. Not a big deal, but it is something nifty that Python and Swift have supported since the beginning. So, what does it mean? Well, now you can use a negative index to access items from the end of the Array. Instead of using `myArray[myArray.length - 1]` to access the last element of the Array, you can use `myArray[-1]` for the same. 

When I first visited the [MDN doc](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/at) for this feature, The demo did not work as I was still running Chrome 91.x. So I tried writing a quick polyfill for strings. 

````

    function at(index) {
        if (index < 0) {
            return this[this.length + index]
        }
        return this[index]
    }

    if (typeof String.prototype.at === 'undefined') {
        String.prototype.at = at
    }

    console.log("string".at(-1))

````

I then took a look at the one defined in the [proposal](https://github.com/tc39/proposal-relative-indexing-method#polyfill) and noticed a couple of interesting things. 

**`Math.trunc()`** - `myArray[index]` returns undefined when the index is a non-integer whereas, `myArray.at(index)` will ignore the decimal part of the number and consider only its integer part. Also, this is the first time I am seeing an implementation of `Math.trunc()`. Also passing an empty or non-number index returns the first item.

**`Object.defineProperty()`** - Also something I have not used before. So far I have always directly assigned the polyfill to the object's prototype but this looks like a better approach.

And here is the final version of the polyfill after incorporating some changes from the one in the proposal.

````

function at(index) {
    i = Math.trunc(index) || 0
    return i < 0 ? this[this.length + i] : this[i]
}

for (C of [String, Array, Uint8Array, Uint16Array, Uint32Array, Int8Array, Int16Array, Int32Array, Float32Array, Float64Array, BigInt64Array, BigUint64Array]) {
    if (!C.prototype.at) {
        Object.defineProperty(C.prototype, "at",
            {
                value: at,
                writable: true,
                enumerable: false,
                configurable: true
            })
    }
}

````