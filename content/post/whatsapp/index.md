---
title: Auto messages
description: whatsapp auto messages
slug: whatsapp
date: 2024-06-04 00:00:00+0000
image:
categories:
    - Tech
tags:
    - fun
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
draft: True
---

function sendMessage(message){
  const mainEl = document.querySelector('#main')
  const textareaEl = mainEl.querySelector('div[contenteditable="true"]')

  if(!textareaEl) {
    throw new Error('There is no opened conversation')
  }

  textareaEl.focus()
  document.execCommand('insertText', false, message)
  textareaEl.dispatchEvent(new Event('change', { bubbles: true }))

  setTimeout(() => {
    (mainEl.querySelector('[data-testid="send"]') || mainEl.querySelector('[data-icon="send"]')).click()
  }, 100)
}

setInterval(() => sendMessage('Hi'), 1000);
