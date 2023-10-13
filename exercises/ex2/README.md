# Exercise 2 - Build a Cube

In this exercise, we will create a cube. The cube is the heart of our analytical model. It draws together all required measures & dimensions and defines the functionality of the analytical model.

## Exercise 2.1 Build the Basic Cube

**Step 1)** Create your own sub-package like "ZDT187v_[YourInitials]" underneath ZLOCAL.

**Step 2)** Create a Cube View in your package.
New -> Other Repository Object -> Core Data Services -> Data Definition
Click "Next"
Enter the name "ZDT187v_[YourInitials]_Flight_Cube"
Give it the description "Flight Cube"
As Referenced Object, use "/DMO/I_FLIGHT"
Click "Next"
Create a new request "ZDT187v [YourInitials] Flight Hands-On"
Click "Next"
Use the template "Define a View Entity for a Cube" (Prerequisite is that you downloaded and installed the templates)


1. Click here.
<br>![](/exercises/ex2/images/02_01_0010.png)

2.	Insert this line of code.
```abap
response->set_text( |Hello ABAP World! | ). 
```



## Exercise 2.2 Sub Exercise 2 Description

After completing these steps you will have...

1.	Enter this code.
```abap
DATA(lt_params) = request->get_form_fields(  ).
READ TABLE lt_params REFERENCE INTO DATA(lr_params) WITH KEY name = 'cmd'.
  IF sy-subrc = 0.
    response->set_status( i_code = 200
                     i_reason = 'Everything is fine').
    RETURN.
  ENDIF.

```

2.	Click here.
<br>![](/exercises/ex2/images/02_02_0010.png)

## Summary

You've now ...

Continue to - [Exercise 3 - Excercise 3 ](../ex3/README.md)
