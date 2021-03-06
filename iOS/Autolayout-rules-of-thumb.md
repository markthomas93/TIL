# Autolayout cheat sheet

> 1. Whole number multipliers are favored over fractional multipliers.
> 2. Positive constants are favored over negative constants.
> 3. Wherever possible, views should appear in layout order: leading to trailing, top to bottom.
> 4. You should avoid assigning constant sizes to views
> 5. You can always replace a single equality relationship with two inequalities.
> 6. Constraints with a priority of 1000 are required. All other constraints are optional.
> 7. An empty image view does not have an intrinsic content size. As soon as you add an image, though, its intrinsic content size is set to the image’s size.
> 8.  With scrolling enabled, a text view does not have an intrinsic content size. With scrolling disabled, by default the view’s intrinsic content size is calculated based on the size of the text without any line wrapping.
> 9. By default, views use a 250 priority for their content hugging, and a 750 priority for their compression resistance. Therefore, it’s easier to stretch a view than it is to shrink it.
> 10. Whenever possible, use the view’s intrinsic content size in your layout. It lets your layout dynamically adapt as the view’s content changes.
> 11. A common example is a label and text field pair. Typically, you want the text field to stretch to fill the extra space while the label remains at its intrinsic content size. To ensure this, make sure the text field’s horizontal content-hugging priority is lower than the label’s.
> 12. To prevent unwanted stretching, increase the content-hugging priority.
> 13. Baseline constraints work only with views that are at their intrinsic content height. If a view is vertically stretched or compressed, the baseline constraints no longer align properly.
> 14. Setting the stack view’s CHCR priorities has no effect, because the stack view does not have an intrinsic content size.
> 15. The system sets and manages the margins of a view controller’s root view. The top and bottom margins are set to zero points, making it easy to extend content under the bars (if any). The side margins vary depending on how and where the controller is presented, but can be either 16 or 20 points. You cannot change these margins.
> 16. Use stack views wherever possible
> 17. Avoid giving views a fixed height or width.
> 18. Always use leading and trailing constraints instead of right and left.
> 19. For text objects that fill the root view from margin to margin, use the readable content guides instead of the layout margins.
For items that need to fill the root view from edge to edge (for example, background images), use the view’s leading and trailing edges.
> 20. If the view extends under the bars, use the top and bottom margins.If the view does not extend under the bars, constrain the view to the top and bottom layout guides instead.
> 21. When programmatically instantiating views, be sure to set their translatesAutoresizingMaskIntoConstraints property to NO.


Reference:https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/AnatomyofaConstraint.html#//apple_ref/doc/uid/TP40010853-CH9-SW1
