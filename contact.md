---
layout: page
title: contact form
subtitle: Please reach out!
---

<style>
.contact-form-container {
    max-width: 800px; /* Optional: Sets a maximum width for larger screens */
    width: 90%; /* Form container takes 90% of its parent's width */
    margin: 0 auto; /* Centers the form horizontally */
}
input[type="message"],
input[type="email"],
textarea {
    width: 100%; /* Input fields take 100% of their parent's width */
    box-sizing: border-box; /* Includes padding and border in the element's total width */
}
  .border-text {
    color: white;
    background-color: #222222;
    border: 2px solid #44E88C;
    outline: none; 
}
.border-button {
    background: #222222;
    padding: 10px 20px;
    border-radius: 25px;
    border: 1px solid #44E88C;
    color: #44E88C;
    font-weight: 500;
    font-size: 14px;
}
.border-button:hover {
    background: #44E88C;
    padding: 10px 20px;
    border-radius: 25px;
    border: 1px solid #44E88C;
    color: #000000;
    font-weight: 500;
    font-size: 14px;
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
            <br><textarea id="message" name="message" class="border-text" rows="10" required></textarea>
        </div>
        <button type="submit" class="border-button">Send Message</button>
    </form>
</div>