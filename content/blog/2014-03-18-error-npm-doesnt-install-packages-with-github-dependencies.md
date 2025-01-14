+++
title = "Error - npm doesn't install packages with GitHub dependencies"
+++

<p>To set in your node project a dependency from github you just need to include the <strong>&lt;username&gt;/&lt;repository&gt;</strong> on package.json:</p>
{% gist maxcnunes/9618061 package.json %}
<p>If you are getting the error below then probably you don't have git installed in your environment. Install that and should fix this problem.</p>
{% gist maxcnunes/9618061 error.sh %}
