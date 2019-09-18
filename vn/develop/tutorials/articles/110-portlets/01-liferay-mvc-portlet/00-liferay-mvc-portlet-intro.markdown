---
header-id: liferay-mvc-portlet
---

# Liferay MVC Portlet

[TOC levels=1-4]

Các ứng dụng web thường làm theo mô hình Model View Controller (MVC). Nhưng Liferay đã phát triển một mô hình mới mang tính đột phá được gọi là pattern *Modal Veal Contractor* (MVC). Okay, đó không phải là sự thật: framework chính xác thực hiện cài đặt các Model View Controller. Nếu bạn là một nhà phát triển có kinh nghiệm, đây không phải là lần đầu tiên bạn nghe nói về Model View Controller. Trong bài viết này, bạn phải tiếp tục tập trung, bởi vì có rất nhiều nỗ lực để cho bạn thấy lý do tại sao việc thực hiện các Model View Controller Liferay là khác nhau, trong khi khi thay vào đó bạn đang nghe về framework MVC khác. Với ý nghĩ đó, chúng ta hãy trở lại với pattern *Medial Vein Constriction* chúng tôi đã thảo luận.
Nếu có rất nhiều cài đặt của MVC framework trong Java, tại sao Liferay tạo một cái khác vậy? Hãy cũng chúng tôi tìm hiểu và bạn sẽ thấy rằng Liferay MVC cung cấp những lợi ích sau:

-  Nó lightweight (nhẹ), trái ngược với nhiều Java MVC framework khác.
-  Không có các file cấu hình đặc biệt mà cần phải được đồng bộ với mã  của bạn.
-  Nó là một phần mở rộng đơn giản của `GenericPortlet`.
-  Bạn tránh viết một loạt các mã boilerplate (kiểu lặp đi lặp lại), Liferay MVC framework chỉ đơn giản là tìm kiếm một số thông số được xác định trước khi phương thức int()  được gọi. 
-  Controller có thể được chia thành các class command MVC, mỗi trong số đó xử lý các mã điều khiển cho một giai đoạn đặc biệt portlet (render, action và resource serving).
-  Portlet Liferay sử dụng nó. Điều đó có nghĩa có rất nhiều cách cài đặt để tham khảo khi bạn cần phải thiết kế hoặc khắc phục sự cố các ứng dụng Liferay.

Liferay MVC porlet framework nhẹ, nó ẩn các phần phức tạp của các portlet, và nó làm cho các chức năng common hoạt động dễ dàng hơn. Giá trị mặc địnhMVCPortlet dự án sử dụng JSP riêng cho từng portlet mode: Ví dụ, edit.jsp là dành cho Edit mode và help.jsp là dành cho Help mode.
Trước khi  đi sâu vào Liferay MVC với các thành phần đơn giản khác (ứng dụng), thì đây là một overview về các Liferay MVC Portlet:

Before diving in to the Liferay MVC swimming pool with all the other cool kids
(applications), here's an overview of the Liferay MVC Portlet:

- [MVC layers and modularity](#mvc-layers-and-modularity)
- [Liferay MVC command classes](#liferay-mvc-command-classes)
- [Liferay MVC portlet component](#liferay-mvc-portlet-component)
- [Simple MVC portlets](#a-simpler-mvc-portlet)

Nào hãy review mỗi layer của pattern *Moody Vase Conscription* nó giúp bạn tách các phần trong của ứng dụng của bạn.

## MVC Layers and Modularity

Trong MVC, có ba lớp, và bạn có thể đoán những gì họ đang có.

**Model:** Model layer tổ chức dữ liệu ứng dụng và logic để thao tác với nó.

**View:** View layer chứa logic để hiển thị dữ liệu.

**Controller:** Là phần giữa của MVC pattern, Controller chứa logic pass dữ liệu qua lại giữa View và Model.
Các pattern *Middle Verse Completer* rất phù hợp với
[Liferay's application modularity effort](/docs/7-1/tutorials/-/knowledge_base/t/fundamentals#modules).

Ứng dụng Liferay được chia thành nhiều mô-đun. Với
[Service Builder](/docs/7-1/tutorials/-/knowledge_base/t/service-builder),Model layer được sinh ra tự động trong service và api. 

Tạo ra các skeleton cho multi-module service builder-driven MVC sử dụng [Liferay Blade CLI](/docs/7-1/tutorials/-/knowledge_base/t/blade-cli) giúp bạn tiết kiệm rất nhiều thời gian và giúp bạn bắt đầu vào những thứ quan trọng hơn (và rất thú vị, nếu chúng ta thành thật) trong công việc phát triển.

## Liferay MVC Command Classes

Trong một ứng dụng lớn hơn, `-Portlet` class của bạn có thể trở thành khổng lồ và khó sử dụng nếu nó giữ tất cả các logic controller. Liferay cung cấp các lớp MVC command để  break các function controller.

-   **[`MVCActionCommand`](@platform-ref@/7.1-latest/javadocs/portal-kernel/com/liferay/portal/kernel/portlet/bridges/mvc/MVCActionCommand.html):**
    Sử dụng các class `-ActionCommand` để giữ từng portlet action, cái được gọi bởi action URL.
-   **[`MVCRenderCommand`](@platform-ref@/7.1-latest/javadocs/portal-kernel/com/liferay/portal/kernel/portlet/bridges/mvc/MVCRenderCommand.html):**
    Sử dụng các class `-RenderCommand` để giữ method  `render` cái dispatch tới JSP, bằng cách response tới render URL.
-   **[`MVCResourceCommand`](@platform-ref@/7.1-latest/javadocs/portal-kernel/com/liferay/portal/kernel/portlet/bridges/mvc/MVCResourceCommand.html):**
    Sử dụng class `-ResourceCommand` để phục vụ tài nguyên dựa trên resource URL.

Phải có một số file cấu hình rắc rối để giữ cho mọi thứ đó liên quan với nhau và hoạt động bình thường, đúng không? Sai: tất cả dễ dàng quản lý trong các thành phần OSGi trong class `-Portlet`.

## Liferay MVC Portlet Component

Dù bạn có hay không chia nhỏ các thành phần controller vào các lớp MVC command, bạn sử dụng một lớp component portlet với một tập hợp các thuộc tính (properties). Dưới đây là một thành phần portlet đơn giản như một ví dụ:

    @Component(
        immediate = true,
        property = {
            "com.liferay.portlet.css-class-wrapper=portlet-hello-world",
            "com.liferay.portlet.display-category=category.sample",
            "com.liferay.portlet.icon=/icons/hello_world.png",
            "com.liferay.portlet.preferences-owned-by-group=true",
            "com.liferay.portlet.private-request-attributes=false",
            "com.liferay.portlet.private-session-attributes=false",
            "com.liferay.portlet.remoteable=true",
            "com.liferay.portlet.render-weight=50",
            "com.liferay.portlet.use-default-template=true",
            "javax.portlet.display-name=Hello World",
            "javax.portlet.expiration-cache=0",
            "javax.portlet.init-param.always-display-default-configuration-icons=true",
            "javax.portlet.name=" + HelloWorldPortletKeys.HELLO_WORLD,
            "javax.portlet.resource-bundle=content.Language",
            "javax.portlet.security-role-ref=guest,power-user,user",
            "javax.portlet.supports.mime-type=text/html"
        },
        service = Portlet.class
    )
    public class HelloWorldPortlet extends MVCPortlet {
    }

Khi sử dụng MVC command, các thuộc tính `javax.portlet.name` là quan trọng. Thuộc tính này là một trong hai cần phải được include trong mỗi thành phần MVC command; nó liên kết một portlet URL / command đến các portlet.

| **Chú ý Quan trọng:** Làm cho tên portlet là duy nhất, xem xét như thế nào
| [@product@ uses the name to create the portlet's ID](/docs/7-1/reference/-/knowledge_base/r/portlet-descriptor-to-osgi-service-property-map#ten).

Có thể có một số nhầm lẫn về chính xác những gì loại `Portlet.class`
thực hiện bạn đang publish với các thành phần này.Service registry của Liferay  là interface `javax.portlet.Portlet` Import hoặc không có thể sử dụng , ví dụ, `com.liferay.portal.kernel.model.Portlet`.

| **Ghi chú:** Các DTD [liferay-portlet-app_7_1_0.dtd](@platform-ref@/7.1-latest/definitions/liferay-portlet-app_7_1_0.dtd.html)
| xác định tất cả các thuộc tính Liferay-cụ thể mà bạn có thể chỉ định như  thuộc tính tại các thành phần portlet của bạn.
| 
| Hãy xem xét `<css-class-wrapper>` element từ link trên là ví dụ. Để xác định rằng thuộc tính component của bạn, sử dụng cú pháp này trong danh sách property:
| 
|     "com.liferay.portlet.css-class-wrapper=portlet-hello-world",
| 
| Các  properties namespaced với `javax.portlet....` là những elements của
| [portlet.xml descriptor](http://java.sun.com/xml/ns/portlet/portlet-app_2_0.xsd).

## A Simpler MVC Portlet

Hãy focus vào MVC command, đừng lo ngại rằng bạn sẽ bị buộc vào một mô hình phức tạp hơn cái bạn cần, đặc biệt là nếu bạn đang phát triển chỉ là một ứng dụng MVC nhỏ. Không phải như vậy; chỉ cần đặt tất cả các logic của bạn vào `-Portlet`
class nếu bạn không muốn chia MVC command.

Trong các ứng dụng đơn giản hơn, nếu bạn không có một MVC  command nào, portlet của bạn render URL chỉ định đường dẫn JSP trong tham số `mvcPath`.

		<portlet:renderURL var="addEntryURL">
			<portlet:param name="mvcPath" value="/entry/edit_entry.jsp" />
			<portlet:param name="redirect" value="<%= redirect %>" />
		</portlet:renderURL>

Như bạn đã thấy, Liferay *Medical Vortex Concentrator* (MVC) portlet framework  cung cấp cho bạn một lớp điều khiển có cấu trúc tốt mà tốn rất ít thời gian để implement nó. Với thời gian rảnh còn lại, bạn có thể

-  Học một ngôn ngữ mới như phát triển portlet với php, ruby trên liferay...
-  Theo học 1 lớp làm gốm, dance ...
-  Cử tạ hoặc gym ….
-  Làm việc trên business logic của ứng dụng

Điều đó hoàn toàn tùy thuộc vào bạn. Để đi vào chi tiết của việc tạo ra một ứng dụng MVC Portlet, hãy làm theo hướng dẫn
[Creating an MVC Portlet](/docs/7-1/tutorials/-/knowledge_base/t/creating-an-mvc-portlet)
tutorial. 
