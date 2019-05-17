---
title: Spring注解@PathVariable 与 @RequestParam
date: 2019-05-17 16:42:28
tags: Spring注解@PathVariable 与 @RequestParam
---
## @PathVariable 

当使用@RequestMapping URI template 样式映射时， 即 someUrl/{paramId}, 这时的paramId可通过 @Pathvariable注解绑定它传过来的值到方法的参数上。
示例代码：

<!--more-->

@Controller  
@RequestMapping("/owners/{ownerId}")  
public class RelativePathUriTemplateController {  
  
  @RequestMapping("/pets/{petId}")  
  public void findPet(@PathVariable("ownerId") String ownerId, @PathVariable("petId") String petId, Model model) {      
    // implementation omitted   
  }  
}  
上面代码把URI template 中变量 ownerId的值和petId的值，绑定到方法的参数上。若方法参数名称和需要绑定的uri template中变量名称不一致，需要在@PathVariable("name")指定uri template中的名称。

## @RequestParam

当URL使用 someUrl?id=xxxxxx, 这时的id可通过 @RequestParam注解绑定它传过来的值到方法的参数上。
 @RequestMapping("/pets")  
  public void findPet(@RequestParam("id") String id) {      
    // implementation omitted   
}