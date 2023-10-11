# Level 1 Heading

In this exercise, you will get to know the FLIGHT data model you will be working with.

## Level 2 Heading

After completing these steps you will have an understanding about how you can use the FLIGHT model as the basis for analytical data model according to the STAR SCHEMA.

1.	Click here.
<br>![](/exercises/ex0/images/00_00_0010.png)

2.	Insert this code.
``` abap
 DATA(params) = request->get_form_fields(  ).
 READ TABLE params REFERENCE INTO DATA(param) WITH KEY name = 'cmd'.
  IF sy-subrc <> 0.
    response->set_status( i_code = 400
                     i_reason = 'Bad request').
    RETURN.
  ENDIF.
```

## Summary

Now that you have analysed the FLIGHT data model, you know which elements are needed to start building your first analytical entity: The cube.
Continue to - [Exercise 1 - Buld a Cube](../ex1/README.md)
