## **Razorpay Integration**

#### **Razorpay Account**

Step 1:- Create an account in [Razorpay](https://easy.razorpay.com/onboarding/l1/signup?field=MobileNumber).

Step 2:- After successful login, go to the right side of the dashboard and switch to test mode as shown below.

<figure markdown>

![Alt text](https://prmguidelines.github.io/assets/images/razorpay_1.webp)
</figure>


Step 3:- Next, go to the settings menu on the left side of your dashboard and generate the test keys as shown below

<figure markdown>

![Alt text](https://prmguidelines.github.io/assets/images/razorpay_2.webp)
</figure>

```javascript title="Payment Javascript Code" linenums="1" hl_lines="13"
    <script src="https://code.jquery.com/jquery-3.6.1.min.js"></script>
    <script src="https://checkout.razorpay.com/v1/checkout.js"></script>
    <script type="text/javascript">

        function pay_now()
        {
            var name          = ""; // Add Name
            var amount        = $("#totalamount").val(); // Get total amount
            var actual_amount = parseInt(amount) * 100;
            var description   = "Beauty Parlor"; // Add description

            var options = {
                "key": "", // Add API Key
                "amount": actual_amount, 
                "currency": "INR",
                "name": name,
                "description": description,
                "image": "", // Add Image
                "handler": function (response){
                    console.log(response);
                    $.ajax({
                        url: 'api/common.php',
                        'type': 'POST',
                        'data': {
                            'action': 'payment',  // Add API Callback
                            'name':name,
                            'user': "", 
                            'payment_id':response.razorpay_payment_id,
                            'amount':actual_amount,
                            'total':amount,
                        },
                        success:function(data){
                            console.log(data);
                            window.location.href = 'thank_you.php'; // Add redirect page
                        }

                    });
                },
            };

            var rzp1 = new Razorpay(options);
            rzp1.on('payment.failed', function (response){
                    alert(response.error.code);
                    alert(response.error.description);
                    alert(response.error.source);
                    alert(response.error.step);
                    alert(response.error.reason);
                    alert(response.error.metadata.order_id);
                    alert(response.error.metadata.payment_id);
            });

            rzp1.open();
        }
    </script>


```
