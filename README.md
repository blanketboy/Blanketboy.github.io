<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Note Keeper</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h1>Note Keeper</h1>
    <textarea id="noteInput" placeholder="Write your note here..."></textarea>
    <button onclick="saveNote()">Save Note</button>
    <h2>Saved Notes</h2>
    <ul id="notesList"></ul>
  </div>

  <script src="script.js"></script>
</body>
</html>