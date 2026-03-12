<!-- Скрипт для скрытия сообщений -->
    {% if messages %}
    <script>
    document.addEventListener('DOMContentLoaded', function() {
        setTimeout(function() {
            const messages = document.querySelectorAll('.fixed .bg-green-100');
            messages.forEach(function(message) {
                message.classList.add('message-fade-out');
                setTimeout(function() {
                    message.remove();
                }, 500);
            });
        }, 5000);
    });
    </script>
    {% endif %}
    <!-- Global JavaScript -->
    <script>
        // Mobile Menu
        const mobileMenuBtn = document.getElementById('mobileMenuBtn');
        const mobileNav = document.getElementById('mobileNav');
        const mobileCatalogBtn = document.getElementById('mobileCatalogBtn');
        const mobileCatalogMenu = document.getElementById('mobileCatalogMenu');
        
        if (mobileMenuBtn && mobileNav) {
            mobileMenuBtn.addEventListener('click', function() {
                mobileNav.classList.toggle('hidden');
                document.body.classList.toggle('overflow-hidden');
                
                if (mobileNav.classList.contains('hidden')) {
                    mobileCatalogMenu.classList.add('hidden');
                }
            });
        }
        
        if (mobileCatalogBtn && mobileCatalogMenu) {
            mobileCatalogBtn.addEventListener('click', function(e) {
                e.stopPropagation();
                mobileCatalogMenu.classList.toggle('hidden');
            });
        }
        
       
    </script>

<!-- Подключение библиотеки inputmask -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery.inputmask/5.0.8/jquery.inputmask.min.js"></script>

<script>
document.addEventListener('DOMContentLoaded', function() {
    const phoneInput = document.getElementById('phone-input');
    if (phoneInput) {
        // Маска для российских номеров
        Inputmask({
            mask: '+7 (999) 999-99-99',
            placeholder: '_',
            showMaskOnHover: false,
            showMaskOnFocus: true,
            clearIncomplete: true,
            onBeforeMask: function(value, opts) {
                // Очищаем от всех нецифровых символов
                let processedValue = value.replace(/\D/g, '');
                
                // Если номер начинается с 8, заменяем на +7
                if (processedValue.startsWith('8')) {
                    processedValue = '7' + processedValue.substring(1);
                }
                
                // Если номер начинается с 7, добавляем +
                if (processedValue.startsWith('7')) {
                    processedValue = '+' + processedValue;
                }
                
                // Обрезаем до 11 цифр (без +)
                if (processedValue.replace(/\D/g, '').length > 11) {
                    processedValue = processedValue.substring(0, 12); // +7 и 10 цифр
                }
                
                return processedValue;
            }
        }).mask(phoneInput);

        // Валидация при отправке формы
        const form = phoneInput.closest('form');
        if (form) {
            form.addEventListener('submit', function(e) {
                const value = phoneInput.value.replace(/\D/g, '');
                if (value.length !== 11 || !(value.startsWith('7') || value.startsWith('8'))) {
                    e.preventDefault();
                    alert('Пожалуйста, введите корректный номер телефона (11 цифр, начинается с 7 или 8)');
                    phoneInput.focus();
                }
            });
        }
    }
});
</script>

<script>
// Функции для управления количеством товара
function increaseQuantity() {
    const quantityElement = document.getElementById('quantity');
    const formQuantity = document.getElementById('form-quantity');
    let quantity = parseInt(quantityElement.innerText);
    quantityElement.innerText = quantity + 1;
    formQuantity.value = quantity + 1;
}

function decreaseQuantity() {
    const quantityElement = document.getElementById('quantity');
    const formQuantity = document.getElementById('form-quantity');
    let quantity = parseInt(quantityElement.innerText);
    if (quantity > 1) {
        quantityElement.innerText = quantity - 1;
        formQuantity.value = quantity - 1;
    }
}

// Галерея изображений - современная версия
class ProductGallery {
    constructor() {
        this.currentImageIndex = 0;
        this.images = [
            "{{ product.image.url }}",
            {% for image in product.images.all %}
            "{{ image.image.url }}",
            {% endfor %}
        ].filter(Boolean);
        
        this.init();
    }

    init() {
        this.mainImage = document.getElementById('main-product-image');
        this.thumbnails = document.querySelectorAll('.thumbnail');
        this.currentIndexElement = document.getElementById('current-index');
        this.totalImagesElement = document.getElementById('total-images');
        this.galleryContainer = document.querySelector('.bg-light');
        
        this.setupEventListeners();
        this.setupIntersectionObserver();
        this.updateIndicators();
    }

    setupEventListeners() {
        // Обработчики для миниатюр
        this.thumbnails.forEach(thumb => {
            thumb.addEventListener('click', () => this.changeImage(thumb));
            thumb.addEventListener('keypress', (e) => {
                if (e.key === 'Enter' || e.key === ' ') {
                    e.preventDefault();
                    this.changeImage(thumb);
                }
            });
        });

        // Навигация стрелками
        document.getElementById('next-image')?.addEventListener('click', () => this.nextImage());
        document.getElementById('prev-image')?.addEventListener('click', () => this.prevImage());

        // Swipe для мобильных устройств
        this.setupSwipeEvents();

        // Клавиатурная навигация
        document.addEventListener('keydown', (e) => this.handleKeyboardNavigation(e));

        // Автоматическое отображение кнопок наведения
        if (this.galleryContainer) {
            this.galleryContainer.classList.add('group');
        }
    }

    setupSwipeEvents() {
        let touchStartX = 0;
        let touchStartY = 0;

        this.galleryContainer?.addEventListener('touchstart', (e) => {
            touchStartX = e.changedTouches[0].screenX;
            touchStartY = e.changedTouches[0].screenY;
        });

        this.galleryContainer?.addEventListener('touchend', (e) => {
            const touchEndX = e.changedTouches[0].screenX;
            const touchEndY = e.changedTouches[0].screenY;
            const diffX = touchEndX - touchStartX;
            const diffY = touchEndY - touchStartY;

            // Проверяем, что это горизонтальный свайп, а не вертикальный
            if (Math.abs(diffX) > Math.abs(diffY) && Math.abs(diffX) > 50) {
                if (diffX > 0) {
                    this.prevImage();
                } else {
                    this.nextImage();
                }
            }
        });
    }

    setupIntersectionObserver() {
        const options = {
            root: document.querySelector('.overflow-x-auto'),
            rootMargin: '0px',
            threshold: 0.6
        };

        this.observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('thumbnail-visible');
                }
            });
        }, options);

        this.thumbnails.forEach(thumb => this.observer.observe(thumb));
    }

    async changeImage(element) {
        const newIndex = parseInt(element.dataset.index);
        
        if (this.currentImageIndex === newIndex) return;

        // Анимация исчезновения
        this.mainImage.style.opacity = '0';
        this.mainImage.style.transform = 'scale(0.95) translateZ(0)';

        await this.wait(300);

        // Смена изображения
        this.mainImage.src = element.dataset.imageSrc;
        this.mainImage.alt = "{{ product.name }}";

        // Обновление активной миниатюры
        this.updateActiveThumbnail(element);

        // Обновление индекса
        this.currentImageIndex = newIndex;
        this.updateIndicators();

        // Плавная прокрутка к активной миниатюре
        this.scrollToThumbnail(element);

        // Анимация появления
        await this.wait(50);
        this.mainImage.style.opacity = '1';
        this.mainImage.style.transform = 'scale(1) translateZ(0)';
    }

    updateActiveThumbnail(activeElement) {
        this.thumbnails.forEach(thumb => {
            thumb.classList.remove('active', 'border-2', 'border-primary');
            thumb.classList.add('border', 'border-gray-300');
            thumb.style.transform = 'scale(1)';
        });

        activeElement.classList.add('active', 'border-2', 'border-primary');
        activeElement.classList.remove('border', 'border-gray-300');
        activeElement.style.transform = 'scale(1.05)';
    }

    scrollToThumbnail(element) {
        const container = document.querySelector('.overflow-x-auto');
        if (!container) return;

        const thumbRect = element.getBoundingClientRect();
        const containerRect = container.getBoundingClientRect();

        // Проверяем, видна ли миниатюра
        if (thumbRect.left < containerRect.left || thumbRect.right > containerRect.right) {
            element.scrollIntoView({
                behavior: 'smooth',
                inline: 'center',
                block: 'nearest'
            });
        }
    }

    nextImage() {
        if (this.images.length <= 1) return;
        
        const nextIndex = (this.currentImageIndex + 1) % this.images.length;
        const nextThumbnail = Array.from(this.thumbnails).find(thumb => 
            parseInt(thumb.dataset.index) === nextIndex
        );
        
        if (nextThumbnail) {
            this.changeImage(nextThumbnail);
        }
    }

    prevImage() {
        if (this.images.length <= 1) return;
        
        const prevIndex = (this.currentImageIndex - 1 + this.images.length) % this.images.length;
        const prevThumbnail = Array.from(this.thumbnails).find(thumb => 
            parseInt(thumb.dataset.index) === prevIndex
        );
        
        if (prevThumbnail) {
            this.changeImage(prevThumbnail);
        }
    }

    handleKeyboardNavigation(e) {
        if (e.target.tagName === 'INPUT' || e.target.tagName === 'TEXTAREA') return;

        switch(e.key) {
            case 'ArrowRight':
                e.preventDefault();
                this.nextImage();
                break;
            case 'ArrowLeft':
                e.preventDefault();
                this.prevImage();
                break;
            case 'Home':
                e.preventDefault();
                this.changeImage(this.thumbnails[0]);
                break;
            case 'End':
                e.preventDefault();
                this.changeImage(this.thumbnails[this.thumbnails.length - 1]);
                break;
        }
    }

    updateIndicators() {
        if (this.currentIndexElement) {
            this.currentIndexElement.textContent = this.currentImageIndex + 1;
        }
        if (this.totalImagesElement) {
            this.totalImagesElement.textContent = this.images.length;
        }
    }

    wait(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }
}

// Инициализация галереи
document.addEventListener('DOMContentLoaded', function() {
    new ProductGallery();
});
</script>
    
    {% block extra_scripts %}
    <script>
// Создаем URL с помощью Django шаблонного тега
function getCartAddUrl(productId) {
    return `{% url 'cart:cart_add' 123 %}`.replace('123', productId);
}

function getCartRemoveUrl(productId) {
    return `{% url 'cart:cart_remove' 123 %}`.replace('123', productId);
}
// Функция обновления количества товара
function updateCartQuantity(productId, change) {
    const itemElement = document.querySelector(`.cart-item[data-product-id="${productId}"]`);
    const quantityElement = itemElement.querySelector('.item-quantity-display');
    const priceElement = itemElement.querySelector('.item-price');
    const totalElement = itemElement.querySelector('.item-total-price');
    const decreaseBtn = itemElement.querySelector('.decrease-btn');
    
    const currentQuantity = parseInt(quantityElement.textContent);
    const newQuantity = currentQuantity + change;
    const price = parseFloat(priceElement.textContent);
    
    if (newQuantity < 1) return;
    
    // Блокируем кнопки на время запроса
    setButtonsState(itemElement, false);
    
    const formData = new FormData();
    formData.append('quantity', newQuantity);
    formData.append('update', 'true');
    formData.append('csrfmiddlewaretoken', '{{ csrf_token }}');
    
    fetch(getCartAddUrl(productId), {
        method: 'POST',
        body: formData,
        headers: {
            'X-Requested-With': 'XMLHttpRequest',
        }
    })
    .then(response => response.json())
    .then(data => {
        if (data.success) {
            // Обновляем отображение
            quantityElement.textContent = newQuantity;
            totalElement.textContent = (price * newQuantity).toFixed(2);
            
            // Обновляем общие суммы
            updateCartTotals(data.total_items, data.total_price);
            
            // Обновляем кнопки
            if (newQuantity <= 1) {
                decreaseBtn.disabled = true;
            } else {
                decreaseBtn.disabled = false;
            }
            
            showNotification('Количество обновлено', 'success');
        }
    })
    .catch(error => {
        console.error('Error:', error);
        showNotification('Ошибка при обновлении', 'error');
    })
    .finally(() => {
        setButtonsState(itemElement, true);
    });
}

// Функция удаления товара
function removeFromCart(productId) {
    const itemElement = document.querySelector(`.cart-item[data-product-id="${productId}"]`);
    
    if (!confirm('Удалить товар из корзины?')) return;
    
    setButtonsState(itemElement, false);
    
    const formData = new FormData();
    formData.append('csrfmiddlewaretoken', '{{ csrf_token }}');
    
    fetch(getCartRemoveUrl(productId), {
        method: 'POST',
        body: formData,
        headers: {
            'X-Requested-With': 'XMLHttpRequest',
        }
    })
    .then(response => response.json())
    .then(data => {
        if (data.success) {
            // Плавно скрываем элемент
            itemElement.style.opacity = '0';
            itemElement.style.transform = 'translateX(-100px)';
            
            setTimeout(() => {
                itemElement.remove();
                updateCartTotals(data.total_items, data.total_price);
                
                // Если корзина пуста, перезагружаем страницу
                if (data.total_items === 0) {
                    location.reload();
                }
                
                showNotification('Товар удален из корзины', 'success');
            }, 300);
        }
    })
    .catch(error => {
        console.error('Error:', error);
        showNotification('Ошибка при удалении', 'error');
        setButtonsState(itemElement, true);
    });
}

// Функция обновления общих сумм
function updateCartTotals(totalItems, totalPrice) {
    document.getElementById('cart-items-count').textContent = totalItems;
    document.getElementById('cart-total-price').textContent = totalPrice;
    document.getElementById('cart-grand-total').textContent = totalPrice;
    
    // Обновляем счетчик в хедере
    const cartCounters = document.querySelectorAll('.bg-secondary');
    cartCounters.forEach(counter => {
        if (counter.textContent.match(/^\d+$/)) {
            counter.textContent = totalItems;
        }
    });
}

// Функция блокировки кнопок
function setButtonsState(itemElement, enabled) {
    const buttons = itemElement.querySelectorAll('button');
    buttons.forEach(button => {
        button.disabled = !enabled;
    });
    
    if (!enabled) {
        itemElement.style.opacity = '0.6';
    } else {
        itemElement.style.opacity = '1';
    }
}

// Функция показа уведомления
function showNotification(message, type = 'success') {
    const notification = document.createElement('div');
    notification.className = `fixed top-4 right-4 px-6 py-3 rounded-lg shadow-lg z-50 transform transition-all duration-300 ${
        type === 'success' ? 'bg-green-600' : 'bg-red-600'
    } text-white`;
    notification.textContent = message;
    notification.style.opacity = '0';
    notification.style.transform = 'translateY(-100px)';
    
    document.body.appendChild(notification);
    
    setTimeout(() => {
        notification.style.opacity = '1';
        notification.style.transform = 'translateY(0)';
    }, 100);
    
    setTimeout(() => {
        notification.style.opacity = '0';
        notification.style.transform = 'translateY(-100px)';
        setTimeout(() => notification.remove(), 300);
    }, 3000);
}
</script>

<script>
let currentImageIndex = 0;
const totalImages = {{ review_images|length }};
const reviewImages = [
    {% for image_url in review_images %}
        "{{ image_url }}"{% if not forloop.last %},{% endif %}
    {% endfor %}
];

// Открытие галереи
function openGallery(index) {
    if (reviewImages.length === 0) return;
    
    currentImageIndex = index;
    updateModalImage();
    
    const modal = document.getElementById('galleryModal');
    modal.classList.remove('hidden');
    setTimeout(() => {
        modal.classList.add('opacity-100');
        document.body.style.overflow = 'hidden'; // Блокируем скролл страницы
    }, 10);
}

// Закрытие галереи
function closeGallery() {
    const modal = document.getElementById('galleryModal');
    modal.classList.remove('opacity-100');
    setTimeout(() => {
        modal.classList.add('hidden');
        document.body.style.overflow = ''; // Восстанавливаем скролл
    }, 300);
}

// Навигация по галерее
function navigateGallery(direction) {
    if (reviewImages.length === 0) return;
    
    currentImageIndex += direction;
    
    // Зацикливаем навигацию
    if (currentImageIndex < 0) {
        currentImageIndex = totalImages - 1;
    } else if (currentImageIndex >= totalImages) {
        currentImageIndex = 0;
    }
    
    updateModalImage();
}

// Обновление изображения в модальном окне
function updateModalImage() {
    const modalImage = document.getElementById('modalImage');
    const loadingIndicator = document.getElementById('loadingIndicator');
    const currentIndexElement = document.getElementById('currentIndex');
    const totalImagesElement = document.getElementById('totalImages');
    
    // Показываем индикатор загрузки
    loadingIndicator.classList.remove('hidden');
    modalImage.classList.add('opacity-0');
    
    // Предзагрузка изображения
    const img = new Image();
    img.src = reviewImages[currentImageIndex];
    
    img.onload = function() {
        modalImage.src = reviewImages[currentImageIndex];
        modalImage.alt = `Отзыв ${currentImageIndex + 1}`;
        modalImage.classList.remove('opacity-0');
        loadingIndicator.classList.add('hidden');
        
        // Обновляем счетчик
        currentIndexElement.textContent = currentImageIndex + 1;
        totalImagesElement.textContent = totalImages;
    };
    
    img.onerror = function() {
        loadingIndicator.classList.add('hidden');
        modalImage.classList.remove('opacity-0');
    };
}

// Закрытие по клику на фон
document.getElementById('galleryModal').addEventListener('click', function(e) {
    if (e.target === this) {
        closeGallery();
    }
});

// Навигация с помощью клавиатуры
document.addEventListener('keydown', function(e) {
    const modal = document.getElementById('galleryModal');
    if (!modal.classList.contains('hidden')) {
        switch(e.key) {
            case 'Escape':
                closeGallery();
                break;
            case 'ArrowLeft':
                navigateGallery(-1);
                break;
            case 'ArrowRight':
                navigateGallery(1);
                break;
        }
    }
});

// Swipe для мобильных устройств
let touchStartX = 0;
let touchEndX = 0;

document.getElementById('galleryModal').addEventListener('touchstart', function(e) {
    touchStartX = e.changedTouches[0].screenX;
});

document.getElementById('galleryModal').addEventListener('touchend', function(e) {
    touchEndX = e.changedTouches[0].screenX;
    handleSwipe();
});

function handleSwipe() {
    const swipeThreshold = 50;
    const diff = touchEndX - touchStartX;
    
    if (Math.abs(diff) > swipeThreshold) {
        if (diff > 0) {
            navigateGallery(-1); // Свайп вправо - предыдущее изображение
        } else {
            navigateGallery(1); // Свайп влево - следующее изображение
        }
    }
}

// Предзагрузка изображений для плавной навигации
function preloadImages() {
    reviewImages.forEach(src => {
        const img = new Image();
        img.src = src;
    });
}

// Запускаем предзагрузку после загрузки страницы
window.addEventListener('load', preloadImages);
</script>

<script>
document.addEventListener('DOMContentLoaded', function() {
    // Взаимодействие с картой
    const markers = document.querySelectorAll('.map-marker');
    const filters = document.querySelectorAll('.city-filter');
    const partners = document.querySelectorAll('.partner-card');

    markers.forEach(marker => {
        marker.addEventListener('mouseenter', function() {
            const tooltip = this.querySelector('.tooltip');
            tooltip.classList.remove('hidden');
        });

        marker.addEventListener('mouseleave', function() {
            const tooltip = this.querySelector('.tooltip');
            tooltip.classList.add('hidden');
        });

        marker.addEventListener('click', function() {
            const city = this.getAttribute('data-city');
            filterPartnersByCity(city);
            updateActiveFilter(city);
        });
    });

    filters.forEach(filter => {
        filter.addEventListener('click', function() {
            const city = this.textContent.trim();
            if (city === 'Все города') {
                showAllPartners();
                updateActiveFilter('all');
            } else {
                filterPartnersByCity(city);
                updateActiveFilter(city);
            }
        });
    });

    function filterPartnersByCity(city) {
        partners.forEach(partner => {
            const partnerCity = partner.querySelector('p.text-gray-600').textContent;
            if (partnerCity.includes(city)) {
                partner.style.display = 'block';
                setTimeout(() => {
                    partner.style.opacity = '1';
                    partner.style.transform = 'translateY(0)';
                }, 50);
            } else {
                partner.style.opacity = '0';
                partner.style.transform = 'translateY(20px)';
                setTimeout(() => {
                    partner.style.display = 'none';
                }, 300);
            }
        });
    }

    function showAllPartners() {
        partners.forEach(partner => {
            partner.style.display = 'block';
            setTimeout(() => {
                partner.style.opacity = '1';
                partner.style.transform = 'translateY(0)';
            }, 50);
        });
    }

    function updateActiveFilter(city) {
        filters.forEach(filter => {
            if (city === 'all' && filter.textContent.trim() === 'Все города') {
                filter.classList.add('active');
            } else if (filter.textContent.trim() === city) {
                filter.classList.add('active');
            } else {
                filter.classList.remove('active');
            }
        });
    }

    // Плавное появление карточек
    partners.forEach((partner, index) => {
        partner.style.opacity = '0';
        partner.style.transform = 'translateY(20px)';
        partner.style.transition = 'all 0.6s ease';
        
        setTimeout(() => {
            partner.style.opacity = '1';
            partner.style.transform = 'translateY(0)';
        }, 100 + index * 100);
    });
});
</script>
    {% endblock %}