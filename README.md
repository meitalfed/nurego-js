This JS library renders "canonical" pricing page based on the plans published through Nurego service

To use the library, two very simple steps needed

###Step 1
First, include Nurego.js in the page. We recommend putting the script tag in the ```<head>``` tag.
```JavaScript
<script type="text/javascript" src="http://js.nurego.com/v1/lib/js/nurego.js"></script>
```

###Step 2
After the first step, set your api key. Put this code in ```<body>``` tag. You can get api key from your account.
```JavaScript
<script type="text/javascript">
Nurego.setApiKey('API_KEY');
</script>
```
Pricing plans will be rendered automatically on your page.

That's all!

###Advanced
Some advanced installation are shown here.
```JavaScript
<script type="text/javascript">
//Insert plans into specific block.
Nurego.setParam('element_id', 'my_block');
Nurego.setApiKey('API_KEY');
</script>
...
<div id="my_block">
    <!-- Pricing plans will be here. -->
</div>
```
To show plans for a specific distribution channel and/or segment:
```
<script type="text/javascript">
Nurego.setParam('offerings_url', 'http://api.nurego.com/v1/offerings?segment_guid=<SEGMENT_GUID>&distribution_channel=<CHANNEL>&api_key=');
Nurego.setApiKey('<API_KEY');
</script>
```
To learn about segments and distribution channels, have a look at our [documentation](http://nurego.com/documentation).

###Default parameters
You can override parameters by using ```Nurego.SetParam(<key>, <value>)``` function.
```JavaScript
{
    element_id: null, //Id of the DOM element. (string or null)
    theme: 'nr-default', //CSS class for pricing table. (string or null)
    css_url: 'http://js.nurego.com/v1/lib/css/themes.css', //Url to custom CSS file. (string or null)
    select_url: '/?plan_id=', //Url prefix for plan link. (string)
    select_callback: null, //Callback function after selecting plan. (function or null)
    label_price: 'Monthly cost', //Label in Price column. (string)
    label_select: 'Select', //Label on Select button. (string)
    label_feature_on: '<span class="nr-check nr-yes"></span>', //String for enabled option. (string)
    label_feature_off: '<span class="nr-check nr-no"></span>', //String for disabled option. (string)
    label_before_price: '$', //Prefix for price value. (string)
    label_after_price: '', //Suffix for price value. (string)
    time_out: 5 * 1000, //Timeout in milliseconds. (integer)
    loading_class: 'nr-container nr-loading',  //CSS class for loading block. (string)
    error_class: 'nr-notify nr-red', //CSS class for error block. (string)
    warning_class: 'nr-notify nr-yellow', //CSS class for waring block. (string)
    empty_class: 'nr-container nr-empty', //CSS class for empty block. (string)
    price_class: 'nr-price', //CSS class for price block. (string)
    ...
}
```

###Don't like our pricing page, feel free to create your own. 

This is the simple way to query published plans through the JSONP query.
Include the ```segment_guid``` and ```distribution_channel``` parameters 
in the url to fetch data for a specific segment and/or distribution channel.

To learn about creating/managing distribution channels and segments, visit
our [website](http://nurego.com/documentation).

```HTML
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.8.3/jquery.min.js"></script>
</head>

<body>
<script type="text/javascript">

    $(document).ready(function () {
        var url = 'http://api.nurego.com/v1/offerings?segment_guid=<SEGMENT_GUID>&distribution_channel=<CHANNEL>&api_key=<YOUR API KEY>';

        $.ajax({
            url:  url + '&callback=?',
            type: "GET",
            dataType: "jsonp",
            success: getOfferingsCallback
        });

    });

    function getOfferingsCallback(json) {
        if(json.plans.data == undefined){
            $( ".data" ).append( "Error:" + json.error + "<br>");
        }else{
            $( ".data" ).append( "<h2>OFFERINGS</h2><br>");
            $.each(json.iplans.data, function( index, plan ) {
                $( ".data" ).append( "<h4>" + plan.name + "</h4>");
                $.each(planfeatures.data, function( index, feature ) {
                    $( ".data" ).append( feature.name + " : " + ((feature.max_unit > 0) ? feature.max_unit : "yes" ) + "<br>");
                });
            });
        }
    }

</script>
<div class="wrapper">
    Hello
</div>
<div class="data"></div>
</body>
</html>
```

###Signup Integration

By default our signup page performs all steps. But you can configure it to redirect to your own signup page. You can do this by calling:

```
Nurego.setSignupUrl('YOUR SIGNUP URL');
```

After user clicks 'Go Sign Up' button, nurego-js will redirect browser to your signup page with plan_id parameter.

See examples/signup_url for more details.

###Example: signup

This example shows complete signup flow with Nurego. It's suitable if you want to let Nurego handle signup process.

###Example: signup_url

This example shows signup flow with your application's signup URL. It's suitable if you want to use your own signup page.

###Example: update

This example shows update flow with your application's update URL. You can use it as pricing plan table in user's account page.
