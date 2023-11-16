<!DOCTYPE html>
<html>
<head>
  <title>SHA-256 Hashing</title>
</head>
<body>
  <script>
    async function sha256(input) {
      const encoder = new TextEncoder();
      const data = encoder.encode(input);
      const hashBuffer = await crypto.subtle.digest('SHA-256', data);
      const hashArray = Array.from(new Uint8Array(hashBuffer));
      const hashHex = hashArray.map(byte => byte.toString(16).padStart(2, '0')).join('');
      return hashHex;
    }

    const data = 'Hello, SHA-256!';
    sha256(data).then(hashValue => {
      console.log('SHA-256:', hashValue);
    });
  </script>
</body>
</html>
