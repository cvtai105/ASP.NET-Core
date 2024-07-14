- references: https://learn.microsoft.com/en-us/aspnet/core/web-api/?view=aspnetcore-8.0

- ControllerBase:
    - HttpContext
    - Request
    - Response
    - Url
    - User
    - MetadataProvider
    - ControllerContext
    - ...
    - Ok()
    - NotFound()
    - UnAuthorize()
    - Un
    - NoContent()
    - Forbid()
    - BadRequest()
    - SignIn()
    - StatusCode()
    - Created()
    - CreatedAtAction()
    - CreatedAtRoute()
    - Accepted()
    - AcceptedAtAction()
    - AcceptedAtRoute()
    - ...


- [Route]: chỉ định url cho controller action

- [FromBody]
- [FromForm]
- [FromHeader]
- [FromQuery]
- [FromRoute]
- [FromServices]
- [AsParameters]

- [ApiController]:  
    - require routing attribute
    - Tự động repsonse 400
    - suy diễn các tham số
    - suy luận form data, multipart
    - chi tiết lỗi.

- với controller parameters là complex type thì [ApiController] tự động hiểu nó đến từ body
- nếu parameter là IFormFile/IFormFileCollection [ApiController] hiểu rằng request có content-type là multipart/formdata

- [Comsumes(application/type)]: support content-type