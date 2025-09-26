---
layout: page
title: contact form
subtitle: Please reach out!
---

<style>
.contact-section {
  width: 100%;
  max-width: 40rem;
  margin-left: auto;
  margin-right: auto;
  padding: 3rem 1rem;
}

.form-group-container {
  display: grid;
  gap: 1rem;
  margin-top: 2rem;
}

.form-group {
  display: flex;
  flex-direction: column;
}

.form-label {
  margin-bottom: 0.5rem;
  color: #44E88C;
}

.form-input,
.form-textarea {
  padding: 0.5rem;
  border: 1px solid #44E88C;
  background-color: #222222;
  color: white;
  display: flex;
  height: 2.5rem;
  width: 100%;
  border-radius: 0.375rem;
  font-size: 0.875rem;
  line-height: 1.25rem;
}

.form-input::placeholder,
.form-textarea:focus-visible {
  color: #6b7280;
}

.form-input:focus-visible,
.form-textarea:focus-visible {
  outline: 2px solid #2563eb;
  outline-offset: 2px;
}

.form-textarea {
  min-height: 120px;
}

.form-submit {
  width: 100%;
  margin-top: 1.2rem;
  background-color: #3124ca;
  color: #fff;
  padding: 13px 5px;
  border-radius: 0.375rem;
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

<script>
    window.onload = function() {
        // Reset the form fields when the page loads
        document.getElementById("form").reset();
    };
</script>

<section class="contact-section">

  <form class="contact-form" action="https://api.web3forms.com/submit" method="POST">
    <input type="hidden" name="access_key" value="d3bee2f3-1516-44e3-b80d-f3c3e75bbf34" />
    <input type="hidden" name="subject" value="New Contact Form Submission from Web3Forms" />
    <input type="hidden" name="from_name" value="My Website" />
    <input type="hidden" name="redirect" value="https://nanobytesecurity.com/thanks">
    <div class="form-group-container">
      <div class="form-group">
        <label for="name" class="form-label">Name</label>
        <input id="name" name="name" class="form-input" placeholder="Your name" type="text" />
      </div>
      <div class="form-group">
        <label for="email" class="form-label">Email</label>
        <input id="email" name="email" class="form-input" placeholder="Your email" type="email" />
      </div>
      <div class="form-group">
        <label for="message" class="form-label">Message</label>
        <textarea class="form-textarea" id="message" name="message" placeholder="Your message"></textarea>
      </div>
    </div>
    <button class="border-button" type="submit">Send Message</button>
  </form>
</section>