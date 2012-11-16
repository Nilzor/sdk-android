---
title: Getting started with WebView
layout: default
---
WebView
=========
The authorization process for WebView two steps, in the first we login to SPiD in a WebView and then receives a code. The code is then exchanged for a access token which can be used to make requests against SPiD.
Both these steps are done automatically and the client will only have to use the method `getAuthorizationWebView` method as seen below.
{% highlight java %}
WebView webView = null;
try {
    webView = SPiDClient.getInstance().getAuthorizationWebView(context, null, new SPiDExampleWebViewClient(), new LoginListener());
} catch (Exception e) {
    // Handle problem encoding url, likely to be a configuration issue
}
{% endhighlight %}

If the user should go directly to the registration page the following method can be used.
{% highlight java %}
WebView webView = null;
try {
    SPiDClient.getInstance().getRegistrationWebView(context,
                                                    null,
                                                    new SPiDExampleWebViewClient(),
                                                    new LoginListener(context));
} catch (Exception e) {
    // Handle problem encoding url, likely to be a configuration issue
}
{% endhighlight %}

Or the lost password page.
{% highlight java %}
WebView webView = null;
try {
    SPiDClient.getInstance().getLostPasswordWebView(context,
                                                    null,
                                                    new SPiDExampleWebViewClient(),
                                                    new LoginListener(context));
} catch (Exception e) {
    // Han‹dle problem encoding url, likely to be a configuration issue
}
{% endhighlight %}

The `LoginListener` is a implementation of the `SPiDAuthorizationListener` which is the same as shown in "Setting up SPiD".
{% highlight java %}
protected class LoginListener implements SPiDAuthorizationListener {
    private LoginListener() {
        super();
    }
    @Override
    public void onComplete() {
        // Successful login
    }
    @Override
    public void onSPiDException(SPiDException exception) {
        // Handle SPiDException
    }
    @Override
    public void onIOException(IOException exception) {
        // Handle IOException
    }
}
{% endhighlight %}

The `SPiDWebViewClient` can be extended to provide further functionality as seen below.
{% highlight java %}
protected class SPiDExampleWebViewClient extends SPiDWebViewClient {
    public void onPageStarted(WebView view, String url, Bitmap favicon) {
        super.onPageStarted(view, url, favicon);
        // Show loading spinner
    }
    public void onPageFinished(WebView view, String url) {
        super.onPageFinished(view, url);
        // Hide loading spinner
    }
}
{% endhighlight %}

After the login has been completed and the completion handler has been executed you can start making [API requests](using-spid-requests.html "API requests").