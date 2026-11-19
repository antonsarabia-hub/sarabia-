// Contact form validation for The Rugs and Mich Coral Resort mockup.
// Nothing is sent anywhere: this is a front-end demo only.
(function () {
  var fields = {
    name: { input: document.getElementById('f-name'), error: document.getElementById('err-name'),
      valid: function (v) { return v.trim().length >= 2; } },
    email: { input: document.getElementById('f-email'), error: document.getElementById('err-email'),
      valid: function (v) { return /^\S+@\S+\.\S+$/.test(v.trim()); } },
    message: { input: document.getElementById('f-msg'), error: document.getElementById('err-msg'),
      valid: function (v) { return v.trim().length >= 10; } }
  };
  var status = document.getElementById('form-status');
  var sentName = document.getElementById('sent-name');
  var sendBtn = document.getElementById('send-btn');
  var touched = {};

  function check(key) {
    var f = fields[key];
    var bad = !!touched[key] && !f.valid(f.input.value);
    f.error.hidden = !bad;
    f.input.classList.toggle('invalid', bad);
    f.input.setAttribute('aria-invalid', bad ? 'true' : 'false');
    return !bad && f.valid(f.input.value);
  }

  Object.keys(fields).forEach(function (key) {
    var input = fields[key].input;
    input.addEventListener('blur', function () { touched[key] = true; check(key); });
    input.addEventListener('input', function () {
      status.hidden = true;
      if (touched[key]) check(key);
    });
  });

  sendBtn.addEventListener('click', function () {
    var allValid = true;
    Object.keys(fields).forEach(function (key) {
      touched[key] = true;
      if (!check(key)) allValid = false;
    });
    if (allValid) {
      sentName.textContent = fields.name.input.value.trim();
      status.hidden = false;
    } else {
      status.hidden = true;
      var first = Object.keys(fields).filter(function (k) { return fields[k].input.classList.contains('invalid'); })[0];
      if (first) fields[first].input.focus();
    }
  });
})();
