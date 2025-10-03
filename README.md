function updateOrderStatus(orderId, newStatus) {
            const order = orders.find(o => o.id === orderId);
            if (order) {
                order.status = newStatus;
                localStorage.setItem('shamila_orders', JSON.stringify(orders));
                showNotification('وضعیت سفارش به‌روزرسانی شد');
            }
        }

        function deleteOrder(orderId) {
            if (confirm('آیا از حذف این سفارش اطمینان دارید؟')) {
                orders = orders.filter(o => o.id !== orderId);
                localStorage.setItem('shamila_orders', JSON.stringify(orders));
                displayOrders();
                showNotification('سفارش حذف شد');
            }
        }

        function clearAllOrders() {
            if (confirm('آیا از حذف تمام سفارشات اطمینان دارید؟ این عمل قابل بازگشت نیست.')) {
                orders = [];
                localStorage.setItem('shamila_orders', JSON.stringify(orders));
                displayOrders();
                showNotification('تمام سفارشات حذف شدند');
            }
        }

        // Payment method toggle
        document.addEventListener('DOMContentLoaded', function() {
            const paymentRadios = document.querySelectorAll('input[name="payment"]');
            const cardDetails = document.getElementById('card-details');
            
            paymentRadios.forEach(radio => {
                radio.addEventListener('change', function() {
                    if (this.value === 'card') {
                        cardDetails.style.display = 'block';
                    } else {
                        cardDetails.style.display = 'none';
                    }
                });
            });
        });

        // Handle contact form
        function handleContactForm(event) {
            event.preventDefault();
            showNotification('پیام شما با موفقیت ارسال شد!');
            event.target.reset();
        }

        // Show notification
        function showNotification(message, type = 'success') {
            const notification = document.createElement('div');
            const bgColor = type === 'error' ? 'bg-red-500' : 'bg-green-500';
            notification.className = fixed top-4 right-4 ${bgColor} text-white px-6 py-3 rounded-lg shadow-lg z-50 transform transition-all duration-300;
            notification.textContent = message;
            document.body.appendChild(notification);

            // Animate in
            setTimeout(() => {
                notification.style.transform = 'translateY(0)';
            }, 10);

            setTimeout(() => {
                notification.style.transform = 'translateY(-100%)';
                setTimeout(() => {
                    notification.remove();
                }, 300);
            }, 3000);
        }

        // Scroll to products
        function scrollToProducts() {
            document.getElementById('products').scrollIntoView({ behavior: 'smooth' });
        }

        // Contact Info Management Functions
        function openContactEditModal() {
            if (!isAdminLoggedIn) {
                showNotification('برای دسترسی به این بخش ابتدا وارد شوید', 'error');
                return;
            }
            
            // Load current values
            document.getElementById('edit-phone').value = contactInfo.phone;
            document.getElementById('edit-email').value = contactInfo.email;
            document.getElementById('edit-address').value = contactInfo.address;
            document.getElementById('edit-card').value = contactInfo.card;
            document.getElementById('edit-owner').value = contactInfo.owner;
            
            document.getElementById('contact-edit-modal').classList.remove('hidden');
        }

        function closeContactEditModal() {
            document.getElementById('contact-edit-modal').classList.add('hidden');
        }
