Cho mình biết những cách tối ưu performance của ReactJS
Có dùng memo ko? Và nên dùng khi nào (Use memo)?
Mình có một cái state có 10k objects, làm sao để update 1 object tránh mapping lại 10k objects? Object có rất nhiều fields bên trong (name, ID)
Khách trl: Normalize state đấy ra hoặc dùng hàm map của reactjs để lookup
Điều kiện nào để component đấy ở client side hay sever side (NextJS)? Đưa ra VD
Khi manager những cái liên quan đến state làm gì để lift up or down state? Khi nào quyết định state đấy nằm ở global state?
Authentication: Refresh token : Nói về access token or refresh token?
CHuyện gì xảy ra nếu refresh token bị lỗi?
Ví dụ:mình có app đăng nhập từ hqa, nay mình vào lại thấy dashboard khoảng 10 API được call, trong lúc này cả 10 API trả về 401, thì mình cần refresh token. Làm sao để mình tránh dc refresh token 10 lần mà chỉ được 1 lần thôi

Techstack Backend là gì?
Sử dụng framework gì ở NodeJS?
A có 1 list invoices, họ complaint list inv chậm quá, về phía dev mình xử lý như thế nào? Cách tiếp cận là gì? (Khách hỏi về việc xử lý vấn đề)
Query đã được optimize chưa? (vì UV trả lời liên quan đến Query)
Cần cứng NextJS, React, NodeJS

Share about your previous project (navigation, statement, etc)
How do you handle the security?
Have you used Zustand? Can you show the differences between Jotai & Reduct in React native
Have you had the exp with system biometric? Share about this. Did you write directly to native code or you use package?
Have you used Native Wind? What have you used?
How did you handle crashes? 
Test: What is wrong with the test? How you solve it?

If you’re leading NEXTJS in the next project (mobile banking, Fintech), how would you structure the app that you’re building?

If you were building an international money transfer flow in React Native, how would you structure the user journey from quote generation to payment confirmation?

How would you design beneficiary management in a banking app to minimize fraud and user mistakes?

Describe how you would implement a mobile onboarding flow that includes identity verification (KYC), document capture, and liveness detection

What mobile security measures would you consider mandatory for a banking application? (Secure storage, Certificate pinning, Token rotation, Session timeout)

Where would you store access tokens, refresh tokens, and sensitive user data on iOS and Android?

If you’re leading react native in the next project (mobile banking, Fintech), how would you structure the app that you’re building?
- Share the Architecture of the Project

How did you test the platform? Explain the flow, details
=> Can you walk me through how your team tested the application before each release? What was your role in the testing process?
=> How did you verify critical money transfer or payment scenarios?
=> How did you ensure nothing broke after adding new features?

If you build a platform (money transfer), how will you build it?  (overall)
=> Imagine you're starting a new international money transfer application from scratch. How would you approach the overall architecture and development? 
Walk me through the key components you would build.

What about the business process? The features you need to build for the payment?
=> From a user's perspective, what do you think the complete payment or money transfer journey should look like? What are the main features you would expect in such a platform?

Have you worked in Production support?
=> Have you been involved in supporting a live production application? Can you tell me about an incident you handled and how you resolved it?

When you the platform to production, it could be errors happen, what do you need to do to make sure that you can find and fix the issue quickly?
=> Once a banking or payment application is deployed to production, how do you monitor its health and quickly identify issues if something goes wrong?

What are the hardest things that you have encountered with software development?
=>Looking back at your experience, what's been the most challenging technical problem you've had to solve? What made it difficult, and how did you approach it?


=> Tell me about a feature you're most proud of building for a banking or payment application. Why was it challenging, and what was your specific contribution?


Assume this app for business customer, they have different categories roles: Financial, Admin, etc. they all use the platform, so sometime you do money transfer and 1 person has limited transfer limit (boss: 500.000, employee: 200.000,etc). How would you implement that.
What is the second layer you mean? (a job run schedule)
