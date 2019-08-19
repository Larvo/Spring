# Spring
关于spring框架的相关内容

1.spring中的18个注解

@Controller
标识一个该类是Spring MVC controller处理器，用来创建处理http请求的对象

@RestController
Spring4之后加入的注解，原来在@Controller中返回json需要@ResponseBody来配合，如果直接使用@RestController替代@Controller就不需要再配置@ResponseBody，默认返回json格式。

@Service
用于标注业务层组件，就是加入你有一个用注解的方式把这个类注入到spring配置中

@Autowired
用来装配bean，都可以写在字段上或者方法上。默认情况下必须要求依赖对象必须存在，如果要允许null值，可以设置它的required属性为false，例如：@Autowired(required=false)

@RequestMapping
类定义处：提供初步的请求映射信息，相对于WEB应用的根目录；
方法处：提供进一步的细分映射信息，相对于类定义处的URL。

@RequestParam
用于将请求参数区数据映射到功能处理方法的参数上
例如：
        public String test(@RequestParam Integer id){
                return ".....(id)";
        }
 这个id就是要接受从接口传递过来的参数id的值，如果接口传递过来的参数名和你接收的不一致，也可以如下
        public String test(@RequestPaaram(valuw="userid")Integer id)
        {
                return ".....(id)";
        }
 其中userid就是接口传递的参数，id就是映射userid的参数名。
 
 @ModelAttribute
 使用的地方有三种：
 1、标记在方法上。标记在方法上，会在每一个@RequestMapping标注的方法前执行，如果有返回值，则自动将该返回值加入到ModelMap中
 1）在有返回的方法上：当ModelAttribute设置了value，方法返回的值会以这个value为key，以参数接收到的值作为value，存入到Model中，如下方法执行之后，最终相当于model.addAttribute("username",name);加入@ModelAttribute没有自定义value，则相当于model.Attribute("name",name)
        @ModelAttribute(value="username")
        public String test(@RequestParam(requires=false)String name,Model model){
                return name;
        }
 2)在没有返回值的方法上
 需要手动model.add方法
        @ModelAttribute
        public void test(@RequestParam(required=false)Integer age,Model model){
                model.addAttribute("age",age);
        }
  2、标记在方法的参数上。会将客户端传递过来的参数按名称注入到指定对象中，并且会将这个对象自动加入ModelMap中，便于View层使用
        @RequestMapping(value="test")
        public String test(@ModelAttribute("username")String username,
                @ModelAttribute("name")String name,
                @ModelAttribute("age")String age,Model model){
                System.out.println("进入mod2");
                System.out.println("username"+username);
                System.out.println("name"+name);
                System.out.println("age"+age);
                System.out.println("model"+model);
                return ".....";   
        }
        
        在浏览器中访问并加上参数:地址+?name=我是你&age=12,参数会对应到方法的同名参数上。
        
@Cacheable
用来标记缓存查询。可用于方法或者类中，当标记在一个方法上时表示该方法是支持缓存的，当标记在一个类上时则表示该类所有的方法都是支持缓存的。
参数含义：value 名称 @Cacheable(value={"c1","c2"})
key key @Cacheable(value="c1",key="#id")
condition 条件 @Cacheable(value="c1",condition="#id=11")
比如@Cacheable(value="UserCache")标识的是当调用了标记了这个注解的方法时，逻辑默认加上从缓存中获取结果的逻辑，如果缓存中没有数据，则执行用户编写的查询逻辑，查询成功之后，同时放入缓存中。
但凡说到缓存，都是key-value形式的，因此key就是方法中的参数id，value就是查询的结果，而命名空间UserCache是在spring*.xml中定义的。
        @Cacheable(value="UserCache")
        public Account getUserAge(int id){
                //这里不用写缓存逻辑，直接按正常业务逻辑即可
                //缓存通过切面自动切入
                int age=getUser(id);
                return age;
        }
        
@CacheEvict
用来标记要清空缓存的方法，当这个方法被调用后，就会清空缓存
@CacheEvict(value="UserCache")
参数含义:value 名称 @CacheEvict(value={"c1","c2"})
key key @CacheEvict(value="c1",key="#id")
condition 缓存的条件可以为空
allEntries 是否清空所有的缓存内容 @CacheEvict(value="c1",allEntries=true)
beforeInvocation 是否在方法执行前就清空 @CacheEvict(value="c1",beforeInvocation=true)
        
 
 
 
 
 

