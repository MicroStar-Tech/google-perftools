--- src/base/linux_syscall_support.h
+++ src/base/linux_syscall_support.h
@@ -323,6 +323,106 @@ struct dirent64;
           BODY(type, name, "r"(__r0), "r"(__r1), "r"(__r2), "r"(__r3),        \
                "r"(__r4), "r"(__r5));                                         \
         }
+  #elif defined (__x86_64__)
+    #undef _syscall0
+    #undef _syscall1
+    #undef _syscall2
+    #undef _syscall3
+    #undef _syscall4
+    #undef _syscall5
+    #undef _syscall6
+
+    #define __syscall_return(type, res) \
+        do { \
+	    if ((unsigned long)(res) >= (unsigned long)(-127)) { \
+		errno = -(res); \
+		res = -1; \
+	} \
+	return (type) (res); \
+    } while (0)
+
+    #define __syscall_clobber "r11","rcx","memory" 
+    #define __syscall "syscall"
+
+    #define _syscall0(type,name) \
+    type name(void) \
+    { \
+    long __res; \
+    __asm__ volatile (__syscall \
+    	: "=a" (__res) \
+	: "0" (__NR_##name) : __syscall_clobber ); \
+    __syscall_return(type,__res); \
+    }
+
+    #define _syscall1(type,name,type1,arg1) \
+    type name(type1 arg1) \
+    { \
+    long __res; \
+    __asm__ volatile (__syscall \
+	: "=a" (__res) \
+	: "0" (__NR_##name),"D" ((long)(arg1)) : __syscall_clobber ); \
+    __syscall_return(type,__res); \
+    }
+    	                                                                                                   
+    #define _syscall2(type,name,type1,arg1,type2,arg2) \
+    type name(type1 arg1,type2 arg2) \
+    { \
+    long __res; \
+    __asm__ volatile (__syscall \
+    	: "=a" (__res) \
+    	: "0" (__NR_##name),"D" ((long)(arg1)),"S" ((long)(arg2)) : __syscall_clobber ); \
+    __syscall_return(type,__res); \
+    }
+    
+    #define _syscall3(type,name,type1,arg1,type2,arg2,type3,arg3) \
+    type name(type1 arg1,type2 arg2,type3 arg3) \
+    { \
+    long __res; \
+    __asm__ volatile (__syscall \
+    	: "=a" (__res) \
+    	: "0" (__NR_##name),"D" ((long)(arg1)),"S" ((long)(arg2)), \
+    		  "d" ((long)(arg3)) : __syscall_clobber); \
+    __syscall_return(type,__res); \
+    }
+    
+    #define _syscall4(type,name,type1,arg1,type2,arg2,type3,arg3,type4,arg4) \
+    type name (type1 arg1, type2 arg2, type3 arg3, type4 arg4) \
+    { \
+    long __res; \
+    __asm__ volatile ("movq %5,%%r10 ;" __syscall \
+    	: "=a" (__res) \
+    	: "0" (__NR_##name),"D" ((long)(arg1)),"S" ((long)(arg2)), \
+    	  "d" ((long)(arg3)),"g" ((long)(arg4)) : __syscall_clobber,"r10" ); \
+    __syscall_return(type,__res); \
+    } 
+    
+    #define _syscall5(type,name,type1,arg1,type2,arg2,type3,arg3,type4,arg4, \
+    	  type5,arg5) \
+    type name (type1 arg1,type2 arg2,type3 arg3,type4 arg4,type5 arg5) \
+    { \
+    long __res; \
+    __asm__ volatile ("movq %5,%%r10 ; movq %6,%%r8 ; " __syscall \
+    	: "=a" (__res) \
+    	: "0" (__NR_##name),"D" ((long)(arg1)),"S" ((long)(arg2)), \
+    	  "d" ((long)(arg3)),"g" ((long)(arg4)),"g" ((long)(arg5)) : \
+    	__syscall_clobber,"r8","r10" ); \
+    __syscall_return(type,__res); \
+    }
+    
+    #define _syscall6(type,name,type1,arg1,type2,arg2,type3,arg3,type4,arg4, \
+    	  type5,arg5,type6,arg6) \
+    type name (type1 arg1,type2 arg2,type3 arg3,type4 arg4,type5 arg5,type6 arg6) \
+    { \
+    long __res; \
+    __asm__ volatile ("movq %5,%%r10 ; movq %6,%%r8 ; movq %7,%%r9 ; " __syscall \
+    	: "=a" (__res) \
+    	: "0" (__NR_##name),"D" ((long)(arg1)),"S" ((long)(arg2)), \
+    	  "d" ((long)(arg3)), "g" ((long)(arg4)), "g" ((long)(arg5)), \
+    	  "g" ((long)(arg6)) : \
+    	__syscall_clobber,"r8","r10","r9" ); \
+    __syscall_return(type,__res); \
+    }
+    
   #endif
   #if defined(__x86_64__)
     struct msghdr;
