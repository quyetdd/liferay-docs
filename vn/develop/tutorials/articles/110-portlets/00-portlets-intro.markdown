---
header-id: portlets
---

# Portlets

[TOC levels=1-4]

Các ứng dụng web trong Liferay Portal được gọi là portlet. Giống như nhiều ứng dụng web, các portlet xử lý các requests và sinh ra các responses. Trong response, portlet trả về các nội dung (ví dụ, HTML, XHTML) để hiển thị trong các trình duyệt. Một khác biệt quan trọng giữa các portlet và các ứng dụng web khác là các portlet chạy trong một phần của trang web. Khi bạn đang viết một ứng dụng portlet, bạn chỉ cần phải lo về việc ứng dụng: các trang điều hướng, phần top banner, và bất kỳ thành phần chung nào khác của giao diện được xử lý bởi các thành phần khác. Khác biệt nữa là các portlet chỉ chạy trong portal server. Do đó có thể sử dụng các portlet hỗ trợ hiện có của portal cho quản lý người dùng, authentication, permission, quản lý trang, và các phần khác nữa. Điều này giúp bạn tập trung vào phát triển chức năng cốt lõi của portlet. Trong nhiều trường hợp, cách viết ứng dụng bằng portlet dễ hơn viết ứng dụng độc lập.

Portlet có thể được đặt trên các trang của người dùng (nếu họ có quyền) hoặc portal administrator là những người có thể đặt các portlet khác nhau trên một trang duy nhất. Ví dụ, một trang trong một community site có thể có portlet calendar cho các sự kiện cộng đồng, một portlet thông báo dùng cho các thông báo quan trọng, và một portlet bookmark cho các liên kết quan tâm đến community. Do portal điều khiển layout của trang, nên bạn có thể đặt lại vị trí và thay đổi kích thước một hoặc nhiều portlet trên một trang mà không cần thay đổi bất kỳ đoạn code nào trong portlet. Để làm tất cả điều này trong các ứng dụng web có kiểu khác nhau sẽ yêu cầu code lại hoặc thêm các prolet khác thủ công. Ngoài ra, một portlet duy nhất có thể đảm nhận một trang nếu đó là ứng dụng duy nhất (single page) mà bạn cần trên trang đó. Ví dụ, một message boards hoặc wiki portlet là thích hợp nhất trên trang riêng của mình. Nói tóm lại, các portlet giảm bớt rất nhiều các hạn chế gắn liền với việc phát triển các ứng dụng web truyền thống.

![Hình 1: Bạn có thể đặt nhiều portlet trên một trang duy nhất.](../../images/portlet-applications.png)

Hơn nữa, các portal và các portlet được dựa trên các tiêu chuẩn cơ bản. Năm 2003, Java Portlet Specification 1.0
([JSR-168](https://jcp.org/en/jsr/detail?id=168)) 
đầu tiên xác định behavior của portal và portlet. Năm 2008, Java Portlet Specification 2.0
([JSR-286](https://jcp.org/en/jsr/detail?id=286)) 
được làm mịn và xây dựng dựa trên JSR-168, trong khi vẫn duy trì tính tương thích ngược, để xác định các tính năng như thông tin liên lạc giữa các portlet (IPC-inter-porlet communication) và nhiều thứ khác. Gần đây có Release Java Portlet Specification 3.0
([JSR-362](https://jcp.org/en/jsr/detail?id=362)) 
tiếp tục cải tiến và cải thiện portal và portlet. Liferay dẫn đầu trong lĩnh vực này bằng việc có một thành viên trong nhóm chuyên gia tạo ra các chuẩn đó.
Vậy điều gì làm các đặc tả được xác định? Các links ở  trên cho thấy các định nghĩa hoàn chỉnh; ở đây chúng tôi sẽ tóm tắt ngắn gọn về cách các portlet khác với các servlet-based.
Portlet xử lý các request trong nhiều phase (giai đoạn). Điều này làm cho các portlet linh hoạt hơn nhiều so với servlets. Mỗi giai đoạn portlet thực hiện các hoạt động khác nhau:

- **Render:** Tạo ra nội dung của portlet dựa trên trạng thái hiện hành của
  portlet. Khi giai đoạn này chạy trên một portlet, nó cũng chạy trên tất cả các portlet khác trên trang. Render phase chạy khi bất kỳ portlet trên trang hoàn thành các phase Action hoặc Event. (tức Action , Event xong thì mới đến Render).
- **Action:** Để đáp lại hành động của người dùng, giai đoạn Action thực hiện một
  số hoạt động thay đổi trạng thái của portlet. Giai đoạn Action cũng có thể kích hoạt Event được xử lý bởi các giai đoạn Event nào đó. Sau khi giai đoạn Action và giai đoạn Event nào đó, giai đoạn Render sau đó tạo ra nội dung của portlet. 
- **Event:** Xử lý các sự kiện kích hoạt trong giai đoạn Action. Các Event được
  sử dụng để giao tiếp giữa các quá trình (IPC). Một khi các portlet xử lý tất cả các sự kiện, portal gọi Render phase tất cả các portlet trên trang.
- **Resource-serving:** Phục vụ một nguồn tài nguyên độc lập với phần còn lại của
  lifecycle. Điều này cho phép một portlet phục vụ nội dung động mà không cần chạy giai đoạn Render của tất cả các portlet trên một trang. Giai đoạn Resource-serving xử lý và handle các request AJAX.

So với servlet, portlet cũng có một số khác biệt quan trọng khác. Portlet chỉ làm một phần của một trang, các thẻ như <Html>, <Head>và <Body>không được phép. Do bạn không biết trang của portlet một cách tổng quát, bạn không thể tạo URL portlet trực tiếp. Thay vào đó, API portlet cung cấp cho bạn phương pháp để tạo URL portlet programmatically . Ngoài ra, vì các portlet không có quyền truy cập trực tiếp đến javax.servlet.ServletRequest, Họ không thể đọc được các thông số truy vấn trực tiếp từ một URL. Portlet thay vì truy cập vào một đối tượng javax.portlet.PortletRequest. Trong đặc tả kỹ thuật portlet chỉ cung cấp một cơ chế để một portlet đọc các thông số URL riêng của mình hoặc portlet khác khi portlet khác có khai báo là public quyền truy cập các param. Liferay Portal có cái đó, ngòa ra còn cung cấp method utility để có thể truy cập ServletRequest và các query params. Ngoài ra Portlet cũng có một portlet filter sẵn cho mỗi giai đoạn trong vòng đời portlet. Portlet filter cũng tương tự như servlet filter ở chỗ chúng cho phép sửa đổi request và response một cách nhanh chóng.
Portlet cũng có sự khác biệt với servlets  bằng các mode và các window state. Các Mode  của portlet có các function sau:

- **View mode:** Mode chuẩn của portlet. Sử dụng mode này để truy cập các chức năng chính của portlet.
- **Edit mode:** Mode cấu hình của portlet. Sử dụng Mode này để cấu hình một giao diện tùy chỉnh hoặc behavior (hành vi). Ví dụ, Edit mode của một portlet thời tiết có thể cho phép bạn chọn một địa điểm để lấy dữ liệu thời tiết.
- **Help mode:** Mode hiển thị thông tin trợ giúp của portlet.

Hầu hết các ứng dụng mới chỉ sử dụng View Mode.

Portlet window state điều khiển không gian một portlet chiếm trên một trang. window state bắt chước behavior của ứng dụng desktop truyền thống:

- **Normal:** Các portlet có thể ở trên một trang có chứa portlet khác. Đây là trạng thái window mặc định. 
- **Maximized:** Các portlet chiếm trọn một trang.
- **Minimized:** Chỉ thanh tiêu đề của portlet hiển thị.

Khi bạn phát triển các portlet, bạn có thể tận dụng tất cả các tính năng được xác định bởi các đặc tả kỹ thuật của portlet. Tùy thuộc vào cách bạn phát triển và đóng gói portlet của bạn, tuy nhiên, nó có thể không có khả năng chạy trên portal container khác. Bây giờ bạn có thể nói rằng, “Chờ một phút! Tôi nghĩ Liferay Portal là tiêu chuẩn phù hợp? Tại sao vậy?”Liferay Portal là đạt tiêu chuẩn, nhưng nó thuyết phục trong cách thiêt kế các API để làm cho các developer phát triển dễ dàng hơn. Ví dụ, Liferay Portal chứa một
[MVC framework](/docs/7-1/tutorials/-/knowledge_base/t/liferay-mvc-portlet) 
làm cho nó đơn giản hơn để thực hiện MVC trong portlet của bạn. Tuy nhiên framework này chỉ có sẵn trong Liferay Portal. Không cần sửa đổi, một portlet có sử dụng framework này sẽ không chạy nếu triển khai tới một portal container khác Liferay. Tuy nhiên cần lưu ý rằng chúng tôi không buộc bạn phải sử dụng MVC framework hoặc bất kỳ các API nào đó của chúng tôi. Ví dụ, bạn có thể phát triển portlet,API của bạn với các framework tiêu chuẩn tuân thủ chặt chẽ vào 1 chuẩn nào đó, và đóng gói nó trong một tập tin WAR, và sau đó triển khai nó trên bất kỳ portal container tiêu chuẩn nào phù hợp.
Liferay Portal cũng chứa một runtime OSGi. Điều này có nghĩa rằng bạn không có để phát triển và triển khai portlet của bạn như là một tập tin WAR truyền thống; bạn có thể làm như vậy như OSGi module để thay thế. Chúng tôi khuyến cáo sau này, vì vậy bạn có thể tận dụng các tính năng vốn có trong mô đun OSGi. Đối với một mô tả chi tiết các tính năng này, vui lòng xem hướng dẫn
[OSGi and Modularity](/docs/7-1/tutorials/-/knowledge_base/t/osgi-and-modularity-for-liferay-6-developers). 
Lưu ý, tuy nhiên, các portlet bạn phát triển như module OSGi sẽ không chạy trên container portlet khác mà thiếu một thời gian chạy OSGi. Mặc dù vậy, các
[advantages of modularity](/docs/7-1/tutorials/-/knowledge_base/t/the-benefits-of-modularity)
rất tuyệt vời mà chúng tôi vẫn khuyên bạn phát triển các portlet của bạn như module OSGi.

Vì vậy, các lợi ích để áp dụng các framework và API Liferay là gì? Có một vài thứ:

- Liferay làm theo design pattern. Bạn hiểu về design pattern thì bạn sẽ càng hiểu hơn Liferay Portal.
- Họ đã có 15 năm phát triển portlet.
- Họ cung cấp nhiều tiện ích mà làm cho sự phát triển dễ dàng hơn và nhanh hơn.
- Họ làm cho các ứng dụng của bạn phù hợp với nhiều hệ thống và các hệ thống có sẵn.
- Nếu cần thiết, chúng dễ dàng migrate, vì các thành phần được xây dựng trên các tiêu chuẩn.

Với những gì đã nói, bạn có thể sử dụng một loạt các công nghệ để phát triển các portlet. Phần này cho bạn thấy làm thế nào để phát triển các portlet bằng cách sử dụng các framework và kỹ thuật sau đây:

- [Liferay's MVCPortlet](/docs/7-1/tutorials/-/knowledge_base/t/liferay-mvc-portlet)
- [Liferay Soy Portlet](/docs/7-1/tutorials/-/knowledge_base/t/liferay-soy-portlet)
- [Spring MVC](/docs/7-1/tutorials/-/knowledge_base/t/spring-mvc)
- [Making URLs Friendlier](/docs/7-1/tutorials/-/knowledge_base/t/making-urls-friendlier)
- [Automatic Single Page Applications](/docs/7-1/tutorials/-/knowledge_base/t/automatic-single-page-applications)
- Applying Clay Styles to Your App
- [Creating Layouts Inside Portlets](/docs/7-1/tutorials/-/knowledge_base/t/creating-layouts-inside-custom-portlets)
- [Using JavaScript Inside Portlets](/docs/7-1/tutorials/-/knowledge_base/t/using-javascript-in-your-portlets)

<!-- TODO: readd JSF link, when available. -Cody.
- [JSF Portlets with Liferay Faces](develop/tutorials/-/knowledge_base/7-1/jsf-portlets-with-liferay-faces)
-->

## Related Topics

[Configuring Dependencies](/docs/7-1/tutorials/-/knowledge_base/t/configuring-dependencies)

[Importing Packages](/docs/7-1/tutorials/-/knowledge_base/t/importing-packages)

[Exporting Packages](/docs/7-1/tutorials/-/knowledge_base/t/exporting-packages)
