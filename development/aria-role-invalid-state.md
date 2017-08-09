# Setting the state of invalid

For the question of setting the state of 'invalid' when building interactive web apps, there are two options. Using the class of `.is-invalid` is an easy and common option, but semantically worthless.

HTML5 uses the pseudo element of `:invalid`, while this is better because we don't need to program against this, the fail is that everything is invalid until the user interacts with it when coupled with the attribute of `required` as shown in this [example gist](http://sassmeister.com/gist/9942957). There is also a [JSBin version](http://jsbin.com/xovir/4) you can see too that is better for viewing on a device.

While I am comfortable with the common use case of the class of `.is-invalid` for the presentation of the state, I also advocate for adding all the appropriately semantic attributes.

In the end, for an `invalid` state we could end up with an `<input>` element like the following:

  <input type="text" required class="is-invalid" aria-invalid="true">

If I am understanding the rules correctly:

> Unless an exactly equivalent native attribute is available, host languages SHOULD allow authors to use the aria-required attribute on host language form elements that require input or selection by the user

This tells me that when we are using an `input` element that has a legal attribute of `required` then there is no need to use `aria-required="true"`.

But if there is a case when we may be creating a new element where we are requiring interaction with the user, we would see the following:

  <div class="is-required" aria-required="true"> something to interact with </div>

In the end, these give us all the appropriate hooks for presentation and logic.
