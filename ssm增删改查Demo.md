### mappings/StudentDao.xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

    <mapper namespace="com.li.dao.StudentDao">
      <select id="queryAll" resultType="Student">
        select 
          *
        from
          student
      </select>
      <select id="queryStudentById" parameterType="int" resultType="Student">
        select
          *
        from
          student
        where
          stuid=#{stuid}
      </select>
      <insert id="addStudent" parameterType="Student">
        insert
        into
          student
        values
          (null, #{name}, #{sex})
      </insert>
      <delete id="delStudent" parameterType="int">
        delete
        from
          student
        where
          stuid=#{stuid}	
      </delete>
      <update id="updateStudent" parameterType="Student">
        update
          student
        set
          name=#{name},sex=#{sex}
        where
          stuid=#{stuid}
      </update>
    </mapper>
### StudentDao.java
    package com.li.dao;

    import java.util.List;

    import com.li.pojo.Student;

    public interface StudentDao {

        public List<Student> queryAll();
        public boolean addStudent(Student student);
        public boolean delStudent(int id);
        public boolean updateStudent(Student student);
        public Student queryStudentById(int id);
    }
### StudentService.java
    package com.li.service;

    import java.util.List;

    import com.li.pojo.Student;

    public interface StudentService {

        public List<Student> queryAll();
        public boolean addStudent(Student student);
        public boolean delStudent(int id);
        public boolean updateStudent(Student student);
        public Student queryStudentById(int id);
    }
### StudentServiceImpl.java
    package com.li.service;

    import java.util.List;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Service;

    import com.li.dao.StudentDao;
    import com.li.pojo.Student;

    @Service
    public class StudentServiceImpl implements StudentService{

        @Autowired
        private StudentDao studentDao;

        public List<Student> queryAll() {
            return studentDao.queryAll();
        }
        public boolean addStudent(Student student) {
            return studentDao.addStudent(student);
        }
        public boolean delStudent(int id) {
            return studentDao.delStudent(id);
        }
        public boolean updateStudent(Student student) {
            return studentDao.updateStudent(student);
        }
        public Student queryStudentById(int id) {
            return studentDao.queryStudentById(id);
        }

    }
### StudentController.java
    package com.li.controller;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RequestMethod;
    import org.springframework.web.servlet.ModelAndView;

    import com.li.pojo.Student;
    import com.li.service.StudentService;

    @Controller
    public class StudentController {

        @Autowired
        private StudentService studentService;
        // 跳转页面
        @RequestMapping(value = "jump", method = RequestMethod.GET)
        public String jump(String filename) {
            return filename;
        }
        // 查询
        @RequestMapping("queryAllStudents")
        public ModelAndView queryAllStudents() {
            ModelAndView mView = new ModelAndView();
            mView.addObject("student", studentService.queryAll());
            mView.setViewName("list_student");
            return mView;
        }
        // 添加
        @RequestMapping("addStudent")
        public ModelAndView addStudent(Student student) {
            studentService.addStudent(student);
            return new ModelAndView("redirect:/queryAllStudents");
        }
        // 删除
        @RequestMapping(value = "delStudent")
        public ModelAndView delStudent(int id) {
            studentService.delStudent(id);
            return new ModelAndView("redirect:/queryAllStudents");
        }
        // 预修改
        @RequestMapping("beforeUpdate")
        public ModelAndView beforeUpdate(int id) {
            ModelAndView mView = new ModelAndView();
            mView.addObject("student", studentService.queryStudentById(id));
            mView.setViewName("updateStudent");
            return mView;
        }
        // 修改
        @RequestMapping("updateStudent")
        public ModelAndView updateStudent(Student student) {
            studentService.updateStudent(student);
            return new ModelAndView("redirect:/queryAllStudents");
        }

    }
### WEB-INF/jsp/list_student.jsp
    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title>学生列表</title>
    <link rel="stylesheet"
        href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css"
        integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
        crossorigin="anonymous">
    </head>
    <body>
        <table class="table">
            <thead>
                <tr>
                    <th>id</th>
                    <th>姓名</th>
                    <th>性别</th>
                    <th>操作</th>
                </tr>
            </thead>
            <tbody>
                <c:forEach var="stu" items="${student}">
                    <tr>
                        <td>${stu.stuid}</td>
                        <td>${stu.name}</td>
                        <td>${stu.sex}</td>
                        <td>
                            <a href="jump?filename=addStudent">增加</a> 
                            <a href="delStudent?id=${stu.stuid}">删除</a> 
                            <a href="beforeUpdate?id=${stu.stuid}">修改</a>
                        </td>
                    </tr>
                </c:forEach>
            </tbody>
        </table>
    </body>
    </html>
### WEB-INF/jsp/addStudent.jsp
    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>

    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title>添加学生</title>
    <link rel="stylesheet"
        href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css"
        integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
        crossorigin="anonymous">
    </head>
    <body>
        <form action="addStudent" method="post">
            <div class="input-group">
              <span class="input-group-addon" id="name">姓名</span>
              <input type="text" name="name" class="form-control" placeholder="name" aria-describedby="name">
            </div>
            <div class="input-group">
              <span class="input-group-addon" id="sex">性别</span>
              <input type="text" name="sex" class="form-control" placeholder="sex" aria-describedby="sex">
            </div>
            <div class="input-group">
              <input type="submit" class="btn btn-primary" value="添加">
            </div>
        </form>
    </body>
    </html>
### WEB-INF/jsp/updateStudent.jsp
    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>

    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title>修改学生</title>
    <link rel="stylesheet"
        href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css"
        integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
        crossorigin="anonymous">
    </head>
    <body>
        <form action="updateStudent" method="post">
            <div class="input-group">
              <span class="input-group-addon" id="stuid">id</span>
              <input type="text" class="form-control" placeholder="stuid" aria-describedby="stuid" name="stuid" value="${student.stuid }">
            </div>
            <div class="input-group">
              <span class="input-group-addon" id="name">姓名</span>
              <input type="text" class="form-control" placeholder="name" aria-describedby="name" name="name" value="${student.name }">
            </div>
            <div class="input-group">
              <span class="input-group-addon" id="sex">性别</span>
              <input type="text" class="form-control" placeholder="sex" aria-describedby="sex" name="sex" value="${student.sex }">
            </div>
            <div class="input-group">
              <input type="submit" class="btn btn-primary" value="修改">
            </div>
        </form>
    </body>
    </html>
