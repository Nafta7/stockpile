# respond-to - responsive breakpoint manager

## breakpoints

```sass
$breakpoints: (
  small           : (min-width: 768px),
  medium          : (min-width: 992px),
  large           : (min-width: 1200px),
  extra-small-only: (max-width: 767px)
);
```

## mixin
```sass
/// Responsive breakpoint manager
/// @access public
/// @param {String} $breakpoint - Breakpoint
/// @requires $breakpoints
@mixin respond-to($breakpoint) {
  $raw-query: map-get($breakpoints, $breakpoint);

  @if $raw-query {
    $query: if(
      type-of($raw-query) == 'string',
      unquote($raw-query),
      inspect($raw-query)
    );

    @media #{$query} {
      @content;
    }
  } @else {
    @error 'No value found for `#{$breakpoint}`. '
         + 'Please make sure it is defined in `$breakpoints` map.';
  }
}
```

## usage

```sass
.foo {
  color: red;

  @include respond-to(medium) {
    color: blue;
  }
}
```
Taken from: https://sass-guidelin.es/#responsive-web-design-and-breakpoints
