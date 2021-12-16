## SpringBoot学习（一）@Configuration
# 一、SSM中bean的创建



 

 

 

在springboot中可以不写xml文件，改为用@Configuration的方式，告诉Spring boot这是一个配置类 ~~~ 配置文件

//通过注解方式@Bean来给容器中添加组件，类似于在配置文件.xml中配置<bean id="" class=""><property name="name" value="">这种形式
//以方法名作为id,返回类型就是组件类型，以返回值作为组件在容器中的实例
复制代码
package spring.main.spring.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import spring.main.spring.Bean.User;

@Configuration   //告诉Spring boot这是一个配置类 ~~~ 配置文件
public class Myconfig {
    //通过注解方式@Bean来给容器中添加组件，类似于在配置文件.xml中配置<bean id="" class=""><property name="name" value="">这种形式
    //以方法名作为id,返回类型就是组件类型，以返回值作为组件在容器中的实例
    @Bean
    public User user01() {
        return new User("zs",18);
    }
}
复制代码
@Bean可以通过添加特定的名，作为组件名@Bean("ABC")



 



 

 

 

## 二、在主组件中编写mian方法后运行输出组件的内容，注意这里的name值的是对应的bean的name，也就是方法的类名

 

复制代码
package spring.main.spring;


import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;
import spring.main.spring.Bean.User;

@SpringBootApplication
//相当于
//@SpringBootConfiguration
//@EnableAutoConfiguration
//@ComponentScan("spring.main.spring")，
//SpringBootApplication默认是在主程序所在的包下
public class MainSpring {
    public static void main(String[] args) {
        //返回IOE容器
        ConfigurableApplicationContext run = SpringApplication.run(MainSpring.class,args);

        //查看容器内的组件
        String[] names = run.getBeanDefinitionNames();
        for (String name : names) {
            System.out.println(name);
        }

        //从容器中获取组件
        User user1 = run.getBean("user01",User.class);
        User user2 = run.getBean("user01",User.class);
        System.out.println("组件:"+(user2 == user1));

    }
}

复制代码
 

 

 

 三、

配置类中，组件注册的方法，无论外面调用多少次都是拿的容器中的哪个实例,,如果@Configuration(proxyBeanMethods = true)代理对象调用方法,

SpringBoot总会检查这组件在容器中是否有，如果有则不会新创建，保持组件单实例

如果proxyBeanMethods = false    则不是代理对象，每次调用都会产生一个新的对象

 主要用来解决组件依赖

复制代码
//从容器中获取组件
        User user1 = run.getBean("user01",User.class);
        User user2 = run.getBean("user01",User.class);
        System.out.println("组件:"+(user2 == user1));

        Myconfig bean = run.getBean(Myconfig.class);
        System.out.println(bean);
        User user3 = bean.user01();
        User user4 = bean.user01();
        System.out.println("组件:"+(user3 == user4));
复制代码
@Configuration(proxyBeanMethods = true) //Myconfig文件中的注解
 
