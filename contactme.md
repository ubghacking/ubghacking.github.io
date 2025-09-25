---
layout: page
title: contact me
subtitle: 
---

<style>
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
    <h1>Contact Nanobyte</h1>
    <form action="https://formspree.io/f/mldpdljn" method="POST">
        <div class="form-group">
            <br><label for="email">Your Email:</label>
            <br><input type="email" id="email" name="email" required>
        </div>
        <div class="form-group">
            <label for="message">Message:</label>
            <br><textarea id="message" name="message" class="no-resize-textarea" rows="5" cols="50" required></textarea>
        </div>
        <button type="submit" class="border-button">Send Message</button>
    </form>
</div>