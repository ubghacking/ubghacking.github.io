---
layout: page
title: contact me
subtitle: 
---

.border-button {
  background-color: transparent;
  border: 2px solid #007bff; /* Example: 2px solid blue border */
  padding: 10px 20px;
  cursor: pointer;
  /* Optional: Remove default outline on focus */
  outline: none; 
}

.border-button:hover {
  background-color: #007bff;
  color: white;
}

<p>Please fill out this form to get in contact with me.</p>

<div class="container">
    <h1>Contact Us</h1>
    <form action="https://formspree.io/f/mldpdljn" method="POST">
        <div class="form-group">
            <label for="email">Your Email:</label>
            </br><input type="email" id="email" name="email" required>
        </div>
        <div class="form-group">
            <label for="message">Message:</label>
            </br><textarea id="message" name="message" class="no-resize-textarea"  rows="5" cols="50" required></textarea>
        </div>
        <button type="submit" class="border-button">Send Message</button>
    </form>
</div>