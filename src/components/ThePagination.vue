<template>
	<ul class="pagination justify-content-center mt-3">
		<li class="page-item" :class="{ disabled: isPrevButtonDisabled }">
			<button @click="changePage('prev')" class="page-link" :disabled="isPrevButtonDisabled">
				<span aria-hidden="true">&laquo;</span>
				<span class="visually-hidden">Anterior</span>
			</button>
		</li>
		<li v-for="(page, index) in pages" :key="index" class="page-item" :class="{ active: currentPage === page.val }">
			<button @click="setPage(page.val)" class="page-link" :disabled="currentPage === page.val">
				{{ page.text }}
			</button>
		</li>
		<li class="page-item" :class="{ disabled: isNextButtonDisabled }">
			<button @click="changePage('next')" class="page-link" :disabled="isNextButtonDisabled">
				<span aria-hidden="true">&raquo;</span>
				<span class="visually-hidden">Siguiente</span>
			</button>
		</li>
	</ul>
</template>


<script>
export default {
	props: {
		value: {
			type: Number,
			default: 1,
		},
		totalPages: {
			type: Number,
			default: 1,
		},
		visiblePages: {
			type: Number,
			default: 5,
		},

	},
	computed:{
		isPrevButtonDisabled() {
			const value = this.getPositiveValue(this.value);
			return value === 1;
		},
		isNextButtonDisabled() {
			const value = this.getPositiveValue(this.value);
	
			return value === this.getPositiveValue(this.totalPages);
		},
		currentPage() {
			return this.getPositiveValue(this.value);
		},
		pages() {
			const value = this.getPositiveValue(this.value);
			const totalPages = this.getPositiveValue(this.totalPages);
			const visiblePages = this.getPositiveValue(this.visiblePages);
	
			const leftPages = value - visiblePages;
			const rightPages = value + visiblePages + 1;
			const tempPages = [],
				pages = [];
			let previousPage;
	
			for (let i = 1; i <= totalPages; i++) {
				if (
					i === 1 ||
					i === totalPages ||
					(i >= leftPages && i < rightPages)
				) {
					tempPages.push(i);
				}
			}
	
			const middle = Math.floor(tempPages.length / 2);
	
			tempPages.forEach((i, index) => {
				if (previousPage) {
					if (i - previousPage === 2) {
						const val = {
							text: previousPage + 1,
							val: previousPage + 1,
						};
	
						pages.push(val);
					} else if (i - previousPage !== 1) {
						const val = {
							text: '...',
							val: index < middle ? i - 1 : previousPage + 1,
						};
	
						pages.push(val);
					}
				}
	
				const val = {
					text: i,
					val: i,
				};
	
				pages.push(val);
				previousPage = i;
			});
	
			return pages;
		},
	},
	methods: {
	
		setPage(value) {
			this.$emit('input', value);
			this.$emit('change');
		},
		changePage(go) {
			if (go === 'prev') {
				this.setPage(this.currentPage - 1);
			} else if (go === 'next') {
				this.setPage(this.currentPage + 1);
			}
		},
		getPositiveValue(n) {
			return n > 0 ? n : 1;
		},
	},
}
</script>
<style lang="scss" scoped>
$primary-color: #3f51b5; // Color primario de Material Design

.pagination {
  display: flex;
  justify-content: center;
  list-style: none;
  padding: 0;
  margin-top: 1rem;
}

.page-item {
  margin: 0 4px;

  &.active .page-link {
    background-color: $primary-color;
    color: white;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2); // Sombra de elevación
  }

  &.disabled .page-link {
    color: #9e9e9e;
    pointer-events: none;
  }
}

.page-link {
  display: flex;
  align-items: center;
  justify-content: center;
  min-width: 36px;
  height: 36px;
  border-radius: 50%;
  border: none;
  background-color: transparent;
  color: rgba(0, 0, 0, 0.87);
  font-size: 14px;
  cursor: pointer;
  transition: background-color 0.3s ease, box-shadow 0.3s ease;

  &:hover {
    background-color: rgba(0, 0, 0, 0.08); // Efecto hover
  }

  &:focus {
    outline: none;
  }
}
</style>