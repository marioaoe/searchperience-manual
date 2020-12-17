
## Build product loyalty data for the request

To build product variant loyalty data we are requesting the Price Engine with request values depending on the product variant attribute field `variant.attributes.pointsPayment`. 

If it's set to `true` request values are build with optional attributes:

- `variant.attributes.pointRatio`
- `variant.attributes.pointsPrice`
- `variant.attributes.minPointsRequired`
- `variant.attributes.maxPointsRequired`