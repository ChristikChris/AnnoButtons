
// ==UserScript==
// @name         Paragon Annotation Template Button
// @namespace    http://tampermonkey.net/
// @version      1.9
// @description  Adds a template button in the Annotate & Reply section
// @author       christik@ with a lot of help from Quicksight
// @match        https://paragon*.amazon.com/*
// @match        https://*.paragon*.amazon.com/*
// @match        https://paragon-eu.amazon.com/*
// @match        https://dirc5mvg14a3m.cloudfront.net/*
// @match        https://*.cloudfront.net/*
// @grant        none
// @run-at       document-end
// ==/UserScript==

(function() {
    'use strict';

    console.log('=== Paragon Annotation Template Button v1.9 loaded ===');
    console.log('Current URL:', window.location.href);

    const annotationTemplate = `Batch ID:

Seller intent:

Seller provided identifiers or documents:

Skill:

Research done:

    Change Integration Management (CIM) announcement:
    Paragon workflow (PWF):
    Help page link:
    SOP link:
    Tool link:
    Ticket or SIM reference:

Question to advisor:

Case status:`;

    // Check if we're inside the iframe
    const isInIframe = window.self !== window.top;
    console.log('Running in iframe:', isInIframe);

    function addTemplateButton() {
        // Check if button already exists
        if (document.getElementById('annotation-template-btn')) {
            return true;
        }

        // Find the textarea
        const textarea = document.querySelector('kat-textarea[data-testid="kat-textarea-resolution"]');
        if (!textarea) {
            console.log('Template button: Textarea not found');
            return false;
        }

        console.log('Template button: Found textarea');

        // Find the button container by looking for the action buttons
        let buttonContainer = null;

        // Look for the container with class that contains "action"
        const actionContainers = document.querySelectorAll('[class*="action"]');
        for (const container of actionContainers) {
            const dropdown = container.querySelector('kat-dropdown-button');
            const button = container.querySelector('kat-button');
            if (dropdown && button) {
                buttonContainer = container;
                console.log('Template button: Found button container via action class');
                break;
            }
        }

        // Fallback: traverse up from textarea
        if (!buttonContainer) {
            let currentElement = textarea;
            for (let i = 0; i < 15; i++) {
                currentElement = currentElement.parentElement;
                if (!currentElement) break;

                const allDivs = currentElement.querySelectorAll('div');
                for (const div of allDivs) {
                    const hasDropdown = div.querySelector(':scope > kat-dropdown-button');
                    const hasButton = div.querySelector(':scope > kat-button');
                    if (hasDropdown && hasButton) {
                        buttonContainer = div;
                        console.log('Template button: Found button container via traversal');
                        break;
                    }
                }
                if (buttonContainer) break;
            }
        }

        if (!buttonContainer) {
            console.log('Template button: Button container not found');
            return false;
        }

        console.log('Template button: All elements found, adding button');

        // Create the button
        const templateButton = document.createElement('button');
        templateButton.id = 'annotation-template-btn';
        templateButton.textContent = 'AC Template';
        templateButton.type = 'button';
        templateButton.style.cssText = `
            padding: 8px 16px;
            margin-left: 1px;
            background-color: #5c6f82;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 14px;
            font-weight: 500;
            font-family: "Amazon Ember", Arial, sans-serif;
            height: 32px;
            display: inline-flex;
            align-items: center;
            vertical-align: middle;
        `;

        // Add hover effect
        templateButton.addEventListener('mouseenter', () => {
            templateButton.style.backgroundColor = '#6b7f94';
        });
        templateButton.addEventListener('mouseleave', () => {
            templateButton.style.backgroundColor = '#5c6f82';
        });

        // Add click handler
        templateButton.addEventListener('click', (e) => {
            e.preventDefault();
            e.stopPropagation();
            console.log('Template button clicked');

            // Try to access the shadow root
            const shadowRoot = textarea.shadowRoot;
            if (shadowRoot) {
                const actualTextarea = shadowRoot.querySelector('textarea');
                if (actualTextarea) {
                    // Set the value
                    actualTextarea.value = annotationTemplate;

                    // Trigger multiple events
                    actualTextarea.dispatchEvent(new Event('input', { bubbles: true, composed: true }));
                    actualTextarea.dispatchEvent(new Event('change', { bubbles: true, composed: true }));
                    actualTextarea.dispatchEvent(new KeyboardEvent('keyup', { bubbles: true, composed: true }));
                    actualTextarea.dispatchEvent(new KeyboardEvent('keydown', { bubbles: true, composed: true }));

                    // Also set on parent
                    textarea.value = annotationTemplate;
                    textarea.dispatchEvent(new Event('input', { bubbles: true, composed: true }));
                    textarea.dispatchEvent(new Event('change', { bubbles: true, composed: true }));

                    // Focus the textarea
                    actualTextarea.focus();

                    console.log('Template inserted successfully');
                } else {
                    console.error('Could not find textarea inside shadow root');
                }
            } else {
                console.error('Could not access shadow root');
            }
        });

        // Insert the button
        buttonContainer.appendChild(templateButton);
        console.log('=== Template button added successfully ===');

        return true;
    }

    // Use MutationObserver to watch for changes
    const observer = new MutationObserver((mutations) => {
        if (!document.getElementById('annotation-template-btn')) {
            addTemplateButton();
        }
    });

    // Start observing
    observer.observe(document.body, {
        childList: true,
        subtree: true
    });

    // Initial attempts with delays
    setTimeout(() => addTemplateButton(), 1000);
    setTimeout(() => addTemplateButton(), 2000);
    setTimeout(() => addTemplateButton(), 3000);
    setTimeout(() => addTemplateButton(), 5000);
    setTimeout(() => addTemplateButton(), 7000);

    console.log('Template button: Initialization complete');
})();

