flask-lambda2
====================

One file Flask Lambda wrapper. 

This project was forked from:
https://github.com/techjacker/flask-lambda

Improvements:

* support API gateway payload format version 2 (https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-develop-integrations-lambda.html)


Requirements
------------

* Python 3.6+
* Flask 0.10+


Installation
------------

just copy `flask_lambda.py` to you prject

Usage
-----

Here is an example of what a Flask app using this library would look like:

.. code-block:: python

    from flask_lambda import FlaskLambda

    app = FlaskLambda(__name__)


    @app.route('/foo', methods=['GET', 'POST'])
    def foo():
       data = {
           'form': request.form.copy(),
           'args': request.args.copy(),
           'json': request.json
       }
       return (
           json.dumps(data, indent=4, sort_keys=True),
           200,
           {'Content-Type': 'application/json'}
       )


    if __name__ == '__main__':
        app.run(debug=True)

You can access the original input event and context on the Flask request context:

.. code-block:: python

    from flask import request

    assert request.aws_event['input']['httpMethod'] == 'POST'
    assert request.aws_context.get_remaining_time_in_millis() == 10_000