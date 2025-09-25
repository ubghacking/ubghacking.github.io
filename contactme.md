---
layout: page
title: contact me
subtitle: Please reach out!
---

<style>
  .border-text {
  background-color: transparent;
  border: 2px solid #007bff;
  padding: 10px 20px;
  outline: none; 
}

.border-button {
  background-color: transparent;
  border: 2px solid #007bff;
  padding: 10px 20px;
  cursor: pointer;
  outline: none; 
}

.border-button:hover {
  background-color: #007bff;
  color: white;
}
</style>

<div class="container">
    <form action="https://formspree.io/f/mldpdljn" method="POST">
        <div class="form-group">
            <br><label for="email">Your Email:</label>
            <br><input type="email" id="email" name="email" class="border-text" required>
        </div>
        <div class="form-group">
            <label for="message">Message:</label>
            <br><textarea id="message" name="message" class="border-text" rows="10" cols="70" required></textarea>
        </div>
        <button type="submit" class="border-button">Send Message</button>
    </form>
</div>