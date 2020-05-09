---
description: Magento 2 Product Repository Cheatsheet - Product Repositories usefuls
---

# Product Repository Cheatsheet

#### Import Statement <a id="title_22284_86396"></a>

| `use Magento\Catalog\Api\ProductRepositoryInterface; use Magento\Catalog\Api\Data\ProductInterfaceFactory;` |
| :--- |


#### Constr­uctor <a id="title_22284_86397"></a>

|  `/**   * @var ProductRepositoryInterface   */  protected $productRepositoryInterface;   /**   * @var ProductInterfaceFactory   */  protected $productInterfaceFactory;   /**   * @param ProductRepositoryInterface $productRepositoryInterface   */  public function __construct(      ProductRepositoryInterface $productRepositoryInterface,      ProductInterfaceFactory $productInterfaceFactory,  ) {      $this->productRepositoryInterface = $productRepositoryInterface;      $this->productInterfaceFactory = $productInterfaceFactory;  }` |
| :--- |


#### Product Types <a id="title_22284_86401"></a>

| `\Magento\Catalog\Model\Product\Type::TYPE_SIMPLE  \Magento\Catalog\Model\Product\Type::TYPE_BUNDLE  \Magento\Catalog\Model\Product\Type::TYPE_VIRTUAL  \Magento\Downloadable\Model\Product\Type::TYPE_DOWNLOADABLE  \Magento\ConfigurableProduct\Model\Product\Type\Configurable::TYPE_CODE  \Magento\GroupedProduct\Model\Product\Type\Grouped::TYPE_CODE` |
| :--- |


#### Get info about product by product SKU <a id="title_22284_86402"></a>

| `$product = $this->productRepositoryInterface->get($sku);` |
| :--- |


`/**  
 * @param string $sku  
 * @param bool $editMode  
 * @param int|null $storeId  
 * @param bool $force­Reload  
 * @return \Magen­to­\Cat­alo­g\A­pi­\Dat­a\P­rod­uct­Int­erface  
 * @throws \Magen­to­\Fra­mew­ork­\Ex­cep­tio­n\N­oSu­chE­nti­tyE­xce­ption  
 */`

#### Get info about product by product id <a id="title_22284_86403"></a>

| `$product = $this->productRepositoryInterface->getById($productId);` |
| :--- |


`/**  
 * @param int $productId  
 * @param bool $editMode  
 * @param int|null $storeId  
 * @param bool $force­Reload  
 * @return \Magen­to­\Cat­alo­g\A­pi­\Dat­a\P­rod­uct­Int­erface  
 * @throws \Magen­to­\Fra­mew­ork­\Ex­cep­tio­n\N­oSu­chE­nti­tyE­xce­ption  
 */`

#### Delete by SKU <a id="title_22284_86406"></a>

| `$this->productRepositoryInterface->deleteById($sku);` |
| :--- |


`/**  
 * @param string $sku  
 * @return bool Will returned True if deleted  
 * @throws \Magen­to­\Fra­mew­ork­\Ex­cep­tio­n\N­oSu­chE­nti­tyE­xce­ption  
 * @throws \Magen­to­\Fra­mew­ork­\Ex­cep­tio­n\S­tat­eEx­ception  
 */`

#### Create New Product <a id="title_22284_86412"></a>

| `$product = $this->productInterfaceFactory      ->create()      ->setSku('SKU');  // other required attributes  $product = $this->productRepositoryInterface->save($product);` |
| :--- |


#### Update Existing Product <a id="title_22284_86413"></a>

| `$product = $this->productRepositoryInterface->get('SKU');  $product->setStatus(\Magento\Catalog\Model\Product\Attribute\Source\Status::STATUS_DISABLED);  $updated = $this->productRepositoryInterface->save($product);` |
| :--- |


#### Advanced Search Filter Group <a id="title_22284_86430"></a>

|  `/**   * @var \Magento\Catalog\Api\ProductRepositoryInterface   */  private $productRepository;   /**   * @var \Magento\Framework\Api\SearchCriteriaBuilder   */  private $searchCriteriaBuilder;   /**   * @var \Magento\Framework\Api\FilterBuilder   */  private $filterBuilder;   /**   * @var \Magento\Framework\Api\Search\FilterGroupBuilder   */  private $filterGroupBuilder;   /**   * @var \Magento\Framework\Api\SortOrderFactory   */  private $sortOrder;   /**   * @param \Magento\Catalog\Api\ProductRepositoryInterface $productRepository   * @param \Magento\Framework\Api\SearchCriteriaBuilder $searchCriteriaBuilder   * @param \Magento\Framework\Api\FilterBuilder $filterBuilder   * @param \Magento\Framework\Api\Search\FilterGroupBuilder $filterGroupBuilder   * @param \Magento\Framework\Api\SortOrderFactory $sortOrder   */  public function __construct(      \Magento\Catalog\Api\ProductRepositoryInterface $productRepository,      \Magento\Framework\Api\SearchCriteriaBuilder $searchCriteriaBuilder,      \Magento\Framework\Api\FilterBuilder $filterBuilder,      \Magento\Framework\Api\Search\FilterGroupBuilder $filterGroupBuilder,      \Magento\Framework\Api\SortOrderFactory $sortOrder  ) {      $this->productRepository = $productRepository;      $this->searchCriteriaBuilder = $searchCriteriaBuilder;      $this->filterBuilder = $filterBuilder;      $this->filterGroupBuilder = $filterGroupBuilder;      $this->sortOrder = $sortOrder;  }   [...]   $filter1 = $this->filterBuilder      ->setField("url")      ->setValue("%magento.com")      ->setConditionType("like")      ->create();   $filter2 = $this->filterBuilder      ->setField("store_id")      ->setValue("1")      ->setConditionType("eq")      ->create();   $filterGroup1 = $this->filterGroupBuilder      ->setFilters([$filter1, $filter2])      ->create();   $filter3 = $this->filterBuilder      ->setField("url_type")      ->setValue(1)      ->setConditionType("eq")      ->create();   $filterGroup2 = $this->filterGroupBuilder      ->setFilters([$filter3])      ->create();   $searchCriteria = $this->searchCriteriaBuilder->create();  $searchCriteria->setFilterGroups([$filterGroup1, $filterGroup2]);  $this->productRepository->getList($searchCriteria)->getItems();` |
| :--- |


_The code above creates a Search Criteria with the Filters put together in the following way: \(url like %magen­to.com OR store\_id eq 1\) AND \(url\_type eq 1\)_

\_\_

#### Search Criteria <a id="title_22284_86417"></a>

|  `/**   * @var ProductRepositoryInterface   */  private $productRepositoryInterface;   /**   * @var SearchCriteriaBuilder   */  private $searchCriteriaBuilder;   /**   * @param ProductRepositoryInterface $productRepositoryInterface   * @param SearchCriteriaBuilder $searchCriteriaBuilder   */  public function __construct(      ProductRepositoryInterface $productRepositoryInterface,      SearchCriteriaBuilder $searchCriteriaBuilder  ) {      $this->productRepository = $productRepositoryInterface;      $this->searchCriteriaBuilder = $searchCriteriaBuilder;  }   /**   * Get products by ids   * @param array $productIds   * @return ProductInterface[]   */  public function getProducts(array $productIds): array  {      $this->searchCriteriaBuilder->addFilter('entity_id', $productIds, 'in');      $searchCriteria = $this->searchCriteriaBuilder->create();      $products = $this->productRepositoryInterface->getList($searchCriteria)->getItems();      $count = $products->getTotalCount();      return $products;  }` |
| :--- |


#### Useful constants <a id="title_22284_86419"></a>

| `\Magento\Catalog\Api\Data\ProductInterface::SKU \Magento\Catalog\Api\Data\ProductInterface::NAME \Magento\Catalog\Api\Data\ProductInterface::PRICE \Magento\Catalog\Api\Data\ProductInterface::WEIGHT \Magento\Catalog\Api\Data\ProductInterface::STATUS \Magento\Catalog\Api\Data\ProductInterface::VISIBILITY \Magento\Catalog\Api\Data\ProductInterface::ATTRIBUTE_SET_ID \Magento\Catalog\Api\Data\ProductInterface::TYPE_ID  \Magento\Catalog\Api\Data\ProductInterface::CREATED_AT  \Magento\Catalog\Api\Data\ProductInterface::UPDATED_AT  \Magento\Catalog\Api\Data\ProductInterface::MEDIA_GALLERY  \Magento\Catalog\Api\Data\ProductInterface::TIER_PRICE` |
| :--- |


#### Advanced Search <a id="title_22284_86424"></a>

|  `/**   * @var \Magento\Catalog\Api\ProductRepositoryInterface   */  private $productRepository;   /**   * @var \Magento\Framework\Api\SearchCriteriaBuilder   */  private $searchCriteriaBuilder;   /**   * @var \Magento\Framework\Api\FilterBuilder   */  private $filterBuilder;   /**   * @var \Magento\Framework\Api\SortOrderFactory   */  private $sortOrder;   /**   * @param \Magento\Catalog\Api\ProductRepositoryInterface $productRepository   * @param \Magento\Framework\Api\SearchCriteriaBuilder $searchCriteriaBuilder   * @param \Magento\Framework\Api\FilterBuilder $filterBuilder   * @param \Magento\Framework\Api\SortOrderFactory $sortOrder   */  public function __construct(      \Magento\Catalog\Api\ProductRepositoryInterface $productRepository,      \Magento\Framework\Api\SearchCriteriaBuilder $searchCriteriaBuilder,      \Magento\Framework\Api\FilterBuilder $filterBuilder,      \Magento\Framework\Api\SortOrderFactory $sortOrder  ) {      $this->productRepository = $productRepository;      $this->searchCriteriaBuilder = $searchCriteriaBuilder;      $this->filterBuilder = $filterBuilder;      $this->sortOrder = $sortOrder;  }   /**   * Retrieves products to be sent   * @param array $excludedIds   * @param array $filterIds   * @param int $page   * @param int $pageSize   * @return ProductInterface[]   * @throws \Magento\Framework\Exception\InputException   */  public function retrieve(      array $excludedIds,      array $filterIds,      int $page = 1,      int $pageSize = 100  ) : array {      $this->searchCriteriaBuilder->addFilter('type_id', [          \Magento\Catalog\Model\Product\Type::TYPE_SIMPLE,          \Magento\Downloadable\Model\Product\Type::TYPE_DOWNLOADABLE      ], 'in');       if (count($filterIds)) {          $this->searchCriteriaBuilder->addFilter('entity_id', $filterIds, 'in');      }       if (count($excludedIds)) {          $this->searchCriteriaBuilder->addFilter('entity_id', $excludedIds, 'nin');      }       $sortOrder = $this->sortOrder->setField('updated_at')          ->setDirection(\Magento\Framework\Api\SortOrder::SORT_DESC);      $this->searchCriteriaBuilder->setSortOrders([$sortOrder]);       $this->searchCriteriaBuilder->setCurrentPage($page);      $this->searchCriteriaBuilder->setPageSize($pageSize);      $searchCriteria = $this->searchCriteriaBuilder->create();      return $this->productRepository->getList($searchCriteria)->getItems();  }` |
| :--- |


#### Useful Constants <a id="title_22284_86425"></a>

| `\Magento\Framework\Api\SortOrder::SORT_ASC \Magento\Framework\Api\SortOrder::SORT_DESC` |
| :--- |


#### Print Query String <a id="title_22284_86431"></a>

| `$products = $this->productRepositoryInterface->getList($searchCriteria) echo $products->getSelect()->assemble();` |
| :--- |


