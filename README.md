# GitHub PR squash and merge warning

This violentMonkey browser extension script will display warning on GitHub PRs to avoid 

<kbd>
<img width="1035" alt="image" src="https://github.com/user-attachments/assets/b2ce7644-fa2c-41cd-b17a-b75a1eb14180">
</kbd>

# GitHub PR Warning Script Installation

1. Install Violentmonkey Chrome extension (it's for running script on webpages):
   - [Chrome Web Store Link](https://chrome.google.com/webstore/detail/violentmonkey/jinjaccalgkegednnccohejagnlnfdag)

2. Add the script to violentmonkey:
   - Click the violentmonkey extension icon
   - Select the "+" icon to add a new script  
   - Copy the script in and save it (ctrl/cmd + s)

4. Visit any GitHub PR page and try to merge to see the warning

## The script: 
```js
// ==UserScript==
// @name        GitHub PR Warning for Main Branch
// @namespace   github-pr-warning
// @match       https://github.com/*/pull/*
// @grant       none
// @version     1.1
// @author      Paul Treanor
// @description Adds warnings about squash merging into main branch
// ==/UserScript==

(function() {
	'use strict';

	function updateUI() {
		const mergeMessage = document.querySelector('.merge-message');
		if (mergeMessage && !document.getElementById('pr-warning')) {
			const warningElement = document.createElement('div');
			warningElement.id = 'pr-warning';
			warningElement.innerHTML = '<p style="background-color: red; color: white; font-size: 30px; padding: 10px; text-align: center; margin-bottom: 10px;"> 🤔🤔 DO NOT SQUASH AND MERGE INTO MAIN 🤔🤔</p>';
			mergeMessage.insertBefore(warningElement, mergeMessage.firstChild);
		}

		const squashButton = document.querySelector('.btn-group-squash');
		if (squashButton && !squashButton.dataset.warningAdded) {
			squashButton.textContent = 'Squash and merge  🚨 DO NOT CLICK IF MERGING INTO MAIN 🚨';
			squashButton.dataset.warningAdded = 'true';
		}
	}

	updateUI();

	// Watch for dynamic changes
	const observer = new MutationObserver(() => {
		updateUI();
	});

	observer.observe(document.body, {
		childList: true,
		subtree: true
	});
})();

```
